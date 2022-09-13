---
layout: post
title: MyCoRe - Part 3 - Implement a individual frontend
author: Ralf Eichinger
toc: true
---

In my previous posts

* ["MyCoRe - Part 1 - Installation, Configuration and Usage (on localhost)"]({% post_url topics/server/mycore/2022-06-13-mycore-1-installation %}) 
* ["MyCoRe - Part 2 - Importing and Publishing Objects"]({% post_url topics/server/mycore/2022-07-15-mycore-2-import-objects %})

I described how to install, configure and stop/start MyCoRe Digital Content Repository + MIR Webapp on top of it
and how to import objects into the system.

Despite the fact that MIR offers a good and feature rich user frontend, that can be customized to your needs, see:

* [MIR individualisieren](https://www.mycore.de/documentation/apps/mir/mir_customize/)
* [Maven-Archetype als Basis für neue MyCoRe Anwendungen](https://www.mycore.de/documentation/getting_started/gs_ma_install/)

we want to use our MyCoRe/MIR system as backend for serving our individually implementation readonly frontend webapp (based on Java) for our users.

This might be a too big project to do (lacking a lot of features in the first draft), but let's see how far we get...

## Interfaces and APIs of MIR

The obvious "interfaces" are no real interfaces but the core backend systems and their APIs:

* database:
  * JDBC access (SQL)
* Solr:
  * JSON Request-API, see <https://solr.apache.org/guide/8_0/json-request-api.html> (e.g. curl "http://localhost:8983/solr/mir-main/query?q=*:*")
  * Solr-Client: SolrJ, see <https://solr.apache.org/guide/8_0/using-solrj.html>

MIR itself offers several additional interfaces, some of them (only readonly APIs are listed here that we first focus on):

* [REST-API](https://www.mycore.de/documentation/interfaces/interface_rest/)
* [REST-API v2](https://www.mycore.de/documentation/interfaces/interface_rest_v2/)
* [IIIF API](https://www.mycore.de/documentation/interfaces/interface_iiif_support/)

Additionally we can use the MIR-Java-Libraries itself:

* [Solr-Abfragen über die MyCoRe-Java-API](https://www.mycore.de/documentation/search/search_solr_use/) - at the bottom of the page and [Die MyCoRe-Anfragesprache MCR-QL](https://www.mycore.de/documentation/search/search_query_language/)
* [MyCoRe Solr Support - Library](https://github.com/MyCoRe-Org/mycore/tree/2021.06.x/mycore-solr)

### REST API experiments

Following the advice to use the new (v2) REST API, we start experimenting with this API.
Documentation of v2 API can be found under <https://www.mycore.de/documentation/interfaces/interface_rest_v2/>.
The base URL of our local development API is <http://localhost:8081/mir/api/v2>

Some features (search) are only available in REST v1, so we switch in these cases to v1.
Documentation of v1 API can be found under <https://www.mycore.de/documentation/interfaces/interface_rest/>.

#### Getting list of all objects

Browse <http://localhost:8081/mir/api/v2/objects.json>:

```
[
{"id":"mir_mods_00000001","lastModified":"2022-06-21T15:35:58.700Z"},
{"id":"mir_mods_00000002","lastModified":"2022-07-15T12:24:32.926Z"}
]
```

We get a list of objects with fields "id" and "lastModified". Not too much information, but a start...
(A label to show would be nice for creating a list the user can click on)

#### Getting details of an object

Browse <http://localhost:8081/mir/api/v2/objects/mir_mods_00000002>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<mycoreobject xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="datamodel-mods.xsd" ID="mir_mods_00000002" version="2021.06.1-SNAPSHOT" label="mir_mods_00000002">
  <structure>
    <derobjects class="MCRMetaEnrichedLinkID">
      <derobject inherited="0" xlink:type="locator" xlink:href="mir_derivate_00000001">
        <order>1</order>
        <maindoc>Johann_Wolfgang_von_Goethe-Faust_I.pdf</maindoc>
        <classification classid="derivate_types" categid="content" />
      </derobject>
    </derobjects>
  </structure>
  <metadata>
    <def.modsContainer class="MCRMetaXML" heritable="false" notinherit="true">
      <modsContainer inherited="0">
        <mods:mods xmlns:mods="http://www.loc.gov/mods/v3">
          <mods:typeOfResource>text</mods:typeOfResource>
          <mods:genre type="intern" authorityURI="http://www.mycore.org/classifications/mir_genres" valueURI="http://www.mycore.org/classifications/mir_genres#book" />
          <mods:titleInfo xml:lang="de" xlink:type="simple">
            <mods:title>Faust I</mods:title>
            <mods:subTitle>Der Tragödie erster Teil</mods:subTitle>
          </mods:titleInfo>
          <mods:classification generator="mir_genres2marcgt-mycore" authority="marcgt">book</mods:classification>
          <mods:classification generator="mir_genres2diniPublType-mycore" authorityURI="http://www.mycore.org/classifications/diniPublType" valueURI="http://www.mycore.org/classifications/diniPublType#book" />
          <mods:name type="personal" xlink:type="simple">
            <mods:displayForm>Goethe, Johann Wolfgang von</mods:displayForm>
            <mods:role>
              <mods:roleTerm authority="marcrelator" type="code">aut</mods:roleTerm>
            </mods:role>
            <mods:nameIdentifier type="gnd">118540238</mods:nameIdentifier>
            <mods:namePart type="family">Goethe</mods:namePart>
            <mods:namePart type="given">Johann</mods:namePart>
            <mods:namePart type="given">Wolfgang</mods:namePart>
            <mods:namePart type="given">von</mods:namePart>
          </mods:name>
          <mods:originInfo eventType="publication">
            <mods:dateIssued encoding="w3cdtf">1808</mods:dateIssued>
            <mods:place>
              <mods:placeTerm type="text">Tübingen</mods:placeTerm>
            </mods:place>
            <mods:publisher>Cotta’sche Verlagsbuchhandlung</mods:publisher>
            <mods:edition>1. Ausgabe</mods:edition>
          </mods:originInfo>
          <mods:language>
            <mods:languageTerm authority="rfc5646" type="code">de</mods:languageTerm>
          </mods:language>
          <mods:classification displayLabel="sdnb" authority="sdnb">830</mods:classification>
          <mods:accessCondition type="use and reproduction" xlink:href="http://www.mycore.org/classifications/mir_licenses#odc_pddl" xlink:type="simple" />
        </mods:mods>
      </modsContainer>
    </def.modsContainer>
  </metadata>
  <service>
    <servdates class="MCRMetaISO8601Date">
      <servdate type="createdate" inherited="0">2022-07-15T11:39:01.947Z</servdate>
      <servdate type="modifydate" inherited="0">2022-07-15T12:24:32.926Z</servdate>
    </servdates>
    <servflags class="MCRMetaLangText">
      <servflag type="createdby" inherited="0" form="plain">administrator</servflag>
      <servflag type="modifiedby" inherited="0" form="plain">administrator</servflag>
    </servflags>
    <servstates class="MCRMetaClassification">
      <servstate inherited="0" classid="state" categid="published" />
    </servstates>
  </service>
</mycoreobject>
```

We get back a lot metadata, but also the list of derivate IDs in top section `structure`.

#### Get list of all derivates of an object

Browse <http://localhost:8081/mir/api/v2/objects/mir_mods_00000002/derivates>

```
<derobjects>
  <derobject xmlns:xlink="http://www.w3.org/1999/xlink" inherited="0" xlink:type="locator" xlink:href="mir_derivate_00000001">
    <order>1</order>
    <maindoc>Johann_Wolfgang_von_Goethe-Faust_I.pdf</maindoc>
    <classification classid="derivate_types" categid="content" />
  </derobject>
</derobjects>
```

Ok, information we already got when getting details of an object.

#### Get details of an derivate

Browse <http://localhost:8081/mir/api/v2/objects/mir_mods_00000002/derivates/mir_derivate_00000001>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<mycorederivate xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="datamodel-derivate.xsd" ID="mir_derivate_00000001" version="2021.06.1-SNAPSHOT" order="1">
  <derivate>
    <linkmetas class="MCRMetaLinkID" heritable="false">
      <linkmeta inherited="0" xlink:type="locator" xlink:href="mir_mods_00000002" />
    </linkmetas>
    <internals class="MCRMetaIFS" heritable="false">
      <internal inherited="0" maindoc="Johann_Wolfgang_von_Goethe-Faust_I.pdf" />
    </internals>
    <classifications class="MCRMetaClassification" heritable="false">
      <classification inherited="0" classid="derivate_types" categid="content" />
    </classifications>
  </derivate>
  <service>
    <servdates class="MCRMetaISO8601Date">
      <servdate type="createdate" inherited="0">2022-07-15T11:39:23.747Z</servdate>
      <servdate type="modifydate" inherited="0">2022-07-15T12:24:32.948Z</servdate>
    </servdates>
    <servflags class="MCRMetaLangText">
      <servflag type="createdby" inherited="0" form="plain">administrator</servflag>
      <servflag type="modifiedby" inherited="0" form="plain">administrator</servflag>
    </servflags>
    <servstates class="MCRMetaClassification">
      <servstate inherited="0" classid="state" categid="published" />
    </servstates>
  </service>
</mycorederivate>
```

#### Search for objects

see <https://www.mycore.de/documentation/interfaces/interface_rest/#stellen-einer-suchanfrage>

Browse: <http://localhost:8081/mir/api/v1/search?q=*:*> to get all objects

```xml
<?xml version="1.0" encoding="UTF-8"?>
<response>

<lst name="responseHeader">
  <int name="status">0</int>
  <int name="QTime">1</int>
  <lst name="params">
    <str name="q">*:*</str>
    <str name="wt">xml</str>
  </lst>
</lst>
<result name="response" numFound="5" start="0" maxScore="1.0" numFoundExact="true">
  <doc>
    <str name="objectKind">mycoreobject</str>
    <str name="id">mir_mods_00000001</str>
    <str name="returnId">mir_mods_00000001</str>
    <str name="objectProject">mir</str>
    <str name="objectType">mods</str>
    <arr name="link">
      <str>http://www.mycore.org/classifications/mir_licenses#odc_pddl</str>
    </arr>
    <date name="modified">2022-06-21T15:35:58.700Z</date>
    <date name="created">2022-06-21T15:35:58.694Z</date>
    <str name="modifiedby">administrator</str>
    <str name="createdby">administrator</str>
    <str name="state">submitted</str>
    <int name="derCount">0</int>
    <bool name="worldReadable">true</bool>
    <bool name="worldReadableComplete">false</bool>
    <arr name="category">
      <str>state:submitted</str>
      <str>typeOfResource:text</str>
      <str>mir_genres:book</str>
      <str>marcgt:book</str>
      <str>diniPublType:book</str>
      <str>marcrelator:cre</str>
      <str>marcrelator:aut</str>
      <str>rfc5646:de</str>
      <str>SDNB:500</str>
      <str>mir_licenses:odc_pddl</str>
    </arr>
    <arr name="category.top">
      <str>state:submitted</str>
      <str>typeOfResource:text</str>
      <str>mir_genres:book</str>
      <str>marcgt:book</str>
      <str>diniPublType:book</str>
      <str>marcrelator:cre</str>
      <str>marcrelator:aut</str>
      <str>rfc5646:de</str>
      <str>SDNB:500</str>
      <str>mir_licenses:odc_pddl</str>
    </arr>
    <arr name="mods.title">
      <str>Die</str>
      <str>Die Wunder der Natur</str>
    </arr>
    <str name="mods.title.main">Die Die Wunder der Natur</str>
    <arr name="mods.nameIdentifier">
      <str>gnd:116651172</str>
    </arr>
    <arr name="mods.nameIdentifier.top">
      <str>gnd:116651172</str>
    </arr>
    <arr name="mods.name">
      <str>Heilborn, Adolf</str>
    </arr>
    <arr name="mods.name.top">
      <str>Heilborn, Adolf</str>
    </arr>
    <arr name="mods.author">
      <str>Heilborn, Adolf</str>
    </arr>
    <arr name="mods.place">
      <str>Berlin, Leipzig, Stuttgart, Wien</str>
    </arr>
    <arr name="mods.publisher">
      <str>Deutsches Verlagshaus Bong &amp; Co.</str>
    </arr>
    <arr name="mods.genre">
      <str>book</str>
    </arr>
    <str name="mods.dateIssued">1912</str>
    <int name="mods.yearIssued">1912</int>
    <str name="mods.type">book</str>
    <str name="search_result_link_text">Die Die Wunder der Natur</str>
    <arr name="mods.pindexname">
      <str>Heilborn, Adolf:gnd:116651172</str>
    </arr>
    <arr name="mods.nameByRole.personal.aut">
      <str>Heilborn, Adolf:gnd:116651172</str>
    </arr>
    <str name="mir.toc.title">Die Die Wunder der Natur</str>
    <str name="mir.toc.authors">Heilborn, Adolf</str>
    <long name="_version_">1736259155329548288</long>
    <float name="score">1.0</float></doc>
  <doc>
    <str name="objectKind">mycoreobject</str>
    <str name="id">mir_mods_00000002</str>
    <str name="returnId">mir_mods_00000002</str>
    <str name="objectProject">mir</str>
    <str name="objectType">mods</str>
    <arr name="link">
      <str>mir_derivate_00000001</str>
      <str>http://www.mycore.org/classifications/mir_licenses#odc_pddl</str>
    </arr>
    <date name="modified">2022-07-15T12:24:32.926Z</date>
    <date name="created">2022-07-15T11:39:01.947Z</date>
    <str name="modifiedby">administrator</str>
    <str name="createdby">administrator</str>
    <str name="state">published</str>
    <int name="derCount">1</int>
    <arr name="derivates">
      <str>mir_derivate_00000001</str>
    </arr>
    <bool name="worldReadable">true</bool>
    <bool name="worldReadableComplete">true</bool>
    <arr name="category">
      <str>derivate_types:content</str>
      <str>state:published</str>
      <str>typeOfResource:text</str>
      <str>mir_genres:book</str>
      <str>marcgt:book</str>
      <str>diniPublType:book</str>
      <str>marcrelator:cre</str>
      <str>marcrelator:aut</str>
      <str>rfc5646:de</str>
      <str>SDNB:830</str>
      <str>mir_licenses:odc_pddl</str>
    </arr>
    <arr name="category.top">
      <str>state:published</str>
      <str>typeOfResource:text</str>
      <str>mir_genres:book</str>
      <str>marcgt:book</str>
      <str>diniPublType:book</str>
      <str>marcrelator:cre</str>
      <str>marcrelator:aut</str>
      <str>rfc5646:de</str>
      <str>SDNB:830</str>
      <str>mir_licenses:odc_pddl</str>
    </arr>
    <arr name="mods.title">
      <str>Faust I</str>
      <str>Der Tragödie erster Teil</str>
    </arr>
    <str name="mods.title.main">Faust I</str>
    <str name="mods.title.subtitle">Der Tragödie erster Teil</str>
    <arr name="mods.nameIdentifier">
      <str>gnd:118540238</str>
    </arr>
    <arr name="mods.nameIdentifier.top">
      <str>gnd:118540238</str>
    </arr>
    <arr name="mods.name">
      <str>Goethe, Johann Wolfgang von</str>
    </arr>
    <arr name="mods.name.top">
      <str>Goethe, Johann Wolfgang von</str>
    </arr>
    <arr name="mods.author">
      <str>Goethe, Johann Wolfgang von</str>
    </arr>
    <arr name="mods.place">
      <str>Tübingen</str>
    </arr>
    <arr name="mods.publisher">
      <str>Cotta’sche Verlagsbuchhandlung</str>
    </arr>
    <arr name="mods.genre">
      <str>book</str>
    </arr>
    <str name="mods.dateIssued">1808</str>
    <int name="mods.yearIssued">1808</int>
    <str name="mods.type">book</str>
    <str name="search_result_link_text">Faust I : Der Tragödie erster Teil</str>
    <arr name="mods.pindexname">
      <str>Goethe, Johann Wolfgang von:gnd:118540238</str>
    </arr>
    <arr name="mods.pindexname.published">
      <str>Goethe, Johann Wolfgang von:gnd:118540238</str>
    </arr>
    <arr name="mods.nameByRole.personal.aut">
      <str>Goethe, Johann Wolfgang von:gnd:118540238</str>
    </arr>
    <str name="mir.toc.title">Faust I: Der Tragödie erster Teil</str>
    <str name="mir.toc.authors">Goethe, Johann Wolfgang von</str>
    <long name="_version_">1738421436729524224</long>
    <float name="score">1.0</float></doc>
  <doc>
    <str name="id">mir_mods_00000001-N1002D</str>
    <arr name="category">
      <str>marcrelator:cre</str>
      <str>marcrelator:aut</str>
    </arr>
    <arr name="category.top">
      <str>marcrelator:cre</str>
      <str>marcrelator:aut</str>
    </arr>
    <arr name="mods.nameIdentifier">
      <str>gnd:116651172</str>
    </arr>
    <arr name="mods.name">
      <str>Heilborn, Adolf</str>
    </arr>
    <arr name="mods.name.top">
      <str>Heilborn, Adolf</str>
    </arr>
    <long name="_version_">1736259155329548288</long>
    <float name="score">1.0</float></doc>
  <doc>
    <str name="objectKind">mycorederivate</str>
    <str name="id">mir_derivate_00000001</str>
    <str name="returnId">mir_mods_00000002</str>
    <str name="objectProject">mir</str>
    <str name="objectType">derivate</str>
    <arr name="link">
      <str>mir_mods_00000002</str>
    </arr>
    <date name="modified">2022-07-15T12:24:32.948Z</date>
    <date name="created">2022-07-15T11:39:23.747Z</date>
    <str name="modifiedby">administrator</str>
    <str name="createdby">administrator</str>
    <str name="state">published</str>
    <str name="derivateMaindoc">Johann_Wolfgang_von_Goethe-Faust_I.pdf</str>
    <int name="derivateOrder">1</int>
    <arr name="derivateType">
      <str>content</str>
    </arr>
    <arr name="category">
      <str>derivate_types:content</str>
      <str>state:published</str>
    </arr>
    <arr name="category.top">
      <str>derivate_types:content</str>
      <str>state:published</str>
    </arr>
    <long name="_version_">1738421436687581184</long>
    <float name="score">1.0</float></doc>
  <doc>
    <str name="id">mir_mods_00000002-N10042</str>
    <arr name="category">
      <str>marcrelator:cre</str>
      <str>marcrelator:aut</str>
    </arr>
    <arr name="category.top">
      <str>marcrelator:cre</str>
      <str>marcrelator:aut</str>
    </arr>
    <arr name="mods.nameIdentifier">
      <str>gnd:118540238</str>
    </arr>
    <arr name="mods.name">
      <str>Goethe, Johann Wolfgang von</str>
    </arr>
    <arr name="mods.name.top">
      <str>Goethe, Johann Wolfgang von</str>
    </arr>
    <long name="_version_">1738421436729524224</long>
    <float name="score">1.0</float></doc>
</result>
<lst name="facet_counts">
  <lst name="facet_queries"/>
  <lst name="facet_fields">
    <lst name="mods.genre">
      <int name="book">2</int>
    </lst>
  </lst>
  <lst name="facet_ranges"/>
  <lst name="facet_intervals"/>
  <lst name="facet_heatmaps"/>
</lst>
</response>
```

#### Viewing a digital derivate of an object

MyCoRe offers an viewer for different derivate formats.
We will try to add this to our frontend, too.

Homepage: <https://github.com/MyCoRe-Org/mycore/tree/2021.06.x/mycore-viewer>


## Prototype implementation of frontend

The first experiments with the API have shown that there seem to be enough data accessible for a simple frontend webapp.

We mainly will have to deal with JSON and XML over HTTP.

to be continued...