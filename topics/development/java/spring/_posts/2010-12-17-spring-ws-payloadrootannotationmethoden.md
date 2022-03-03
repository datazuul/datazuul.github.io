---
layout: post
title: Spring WS PayloadRootAnnotationMethodEndpointMapping - One Interceptor config for all is not enough...
author: Ralf Eichinger
toc: true
---

Did you ever want a more fine grained configuration for webservice interceptors of Spring WS PayloadRootAnnotationMethodEndpointMapping?

You like the feature of PayloadRootAnnotationMethodEndpointMapping to map requests to methods in one @Endpoint annotated class (because your business services have the same sort of "grouping")?

Interceptors for all annotated endpoints then are all the same you configured under PayloadRootAnnotationMethodEndpointMapping:

```xml
<bean class="org.springframework.ws.server.endpoint.mapping.PayloadRootAnnotationMethodEndpointMapping">
  <property name="interceptors">
    <list>
      <bean class="org.springframework.ws.server.endpoint.interceptor.PayloadLoggingInterceptor" />
      ...
    </list>
  </property>
...
</bean>
```

Let's assume you want to make some webservices secure: you have to add some security interceptors to the config.

This configuration now is making ALL webservices secure...

But you want just some of them be secure and the rest without the security interceptors? ;-(

Some of you guys will end up using PayloadRootQNameEndpointMapping for secure endpoints and PayloadRootAnnotationMethodEndpointMappingfor insecure ones? Really bad...

Fortunately I had the same problem and found one solution (and only one in the whole google search!) pointing to a weblog entry (<http://www.redlab.be/blog/i-tee/apigeek/java/2010/spring-ws-interceptor/>) that no longer exists: ;-(
But it still remained in Google's cache: ;-)

It is from a guy called "Balder". I took the solution of making a new mapper in conjunction with a new "Interceptors" annotation and refined it to function not only on class level but even additionally on method level (speaking: on webservice request level).

So here are the classes and how to use them for configuring Interceptors on method level:

* Interceptors.java

```java
package com.datazuul.webapps.springws.util;

import java.lang.annotation.Documented;
import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

/**
 * Annotation used by the MethodEndpointInterceptorAdder to add
 * interceptors to the EndpointInvocationChain.
 *
 * @see http://www.redlab.be/blog/i-tee/apigeek/java/2010/spring-ws-interceptor/
 * @author Balder
 */
@Target(value = { ElementType.TYPE, ElementType.METHOD })
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface Interceptors {
  /**
   * @return the qualified names of the interceptors to be added to the
   *         invocation chain, default to an empty array
   */
  String[] qualifiers() default {};
}
```

* MethodEndpointInterceptorAdder.java

```java
package com.datazuul.webapps.springws.util;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.HashSet;
import java.util.Iterator;
import java.util.List;

import org.springframework.context.ApplicationContext;
import org.springframework.core.annotation.AnnotationUtils;
import org.springframework.ws.context.MessageContext;
import org.springframework.ws.server.EndpointInterceptor;
import org.springframework.ws.server.EndpointInvocationChain;
import org.springframework.ws.server.endpoint.MethodEndpoint;
import org.springframework.ws.server.endpoint.mapping.PayloadRootAnnotationMethodEndpointMapping;

/**
 * The MethodEndpointInterceptorAdder checks the
 * {@link MethodEndpoint#getMethod()} Method#getDeclaringClass() and
 * Method#getMethod for the {@link Interceptors} annotation. if the
 * annotation is found and declares interceptors, the beans with the given
 * beanNames will be added to the {@link EndpointInvocationChain} for this
 * call.
 *
 * @see http://www.redlab.be/blog/i-tee/apigeek/java/2010/spring-ws-interceptor/
 * @author Balder, Ralf Eichinger
 */
public class MethodEndpointInterceptorAdder extends PayloadRootAnnotationMethodEndpointMapping {
  /*
   * (non-Javadoc)
   * @see
   * org.springframework.ws.server.endpoint.mapping.AbstractEndpointMapping
   * #createEndpointInvocationChain
   * (org.springframework.ws.context.MessageContext, java.lang.Object,
   * org.springframework.ws.server.EndpointInterceptor[])
   */
  @Override
  protected EndpointInvocationChain createEndpointInvocationChain(final MessageContext messageContext,
    final Object endpoint, EndpointInterceptor[] interceptors) {
    if(endpoint.getClass().equals(MethodEndpoint.class)) {
      MethodEndpoint p = (MethodEndpoint) endpoint;
      Interceptors classInterceptors = AnnotationUtils.findAnnotation(p.getMethod().getDeclaringClass(), Interceptors.class);
      Interceptors methodInterceptors = AnnotationUtils.findAnnotation(p.getMethod(), Interceptors.class);
      HashSet allInterceptorQualifiers = new HashSet();
      if(classInterceptors != null) {
        if(classInterceptors.qualifiers().length > 0) {
          for(String interceptor : classInterceptors.qualifiers()) {
            allInterceptorQualifiers.add(interceptor);
          }
        }
      }
      if(methodInterceptors != null) {
        if(methodInterceptors.qualifiers().length > 0) {
          for(String interceptor : methodInterceptors.qualifiers()) {
            allInterceptorQualifiers.add(interceptor);
          }
        }
      }
      if(allInterceptorQualifiers.size() > 0) {
        ApplicationContext appContext = getApplicationContext();
        List list = new ArrayList();
        Iterator it = allInterceptorQualifiers.iterator();
        while(it.hasNext()) {
          String interceptor = (String) it.next();
          if(appContext.containsBean(interceptor)) {
            Object bean = appContext.getBean(interceptor);
            list.add((EndpointInterceptor) bean);
          }
        }
        if(null != interceptors) {
          list.addAll(Arrays.asList(interceptors));
        }
        interceptors = (EndpointInterceptor[]) list.toArray(new EndpointInterceptor[0]);
      }
    }
    return super.createEndpointInvocationChain(messageContext, endpoint, interceptors);
  }
}
```

Usage in an Endpoint class:

```java
@Endpoint
public class PersonServiceEndpoint {
  public final static String NAMESPACE = "http://datazuul.com/schemas/person";

  @Autowired
  private PersonService personService;

  @PayloadRoot(localPart = "GetPersonRequest", namespace = NAMESPACE)
  @Interceptors(qualifiers={"payloadLoggingInterceptor"})
  // more interceptors separated by ","
  public GetPersonResponse getPerson( GetPersonRequest personRequest ) throws Exception {
    ...
  }
}
```

Change in Spring's configuration:

replace

```xml
<bean class="org.springframework.ws.server.endpoint.mapping.PayloadRootAnnotationMethodEndpointMapping" />
```

with

```xml
<bean class="com.datazuul.webapps.springws.util.MethodEndpointInterceptorAdder" />
```

Here you go! Individual interceptors for each request method.