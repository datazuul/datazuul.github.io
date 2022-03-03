---
layout: post
title: HTML-Grundanweisungen
author: Ralf Eichinger
toc: true
---

# Grundgerüst einer HTML-Seite

<table>
<tr>
<th>Beispiel-Dokument</th>
<th>Beschreibung</th>
</tr>
  
<tr>
<td>

&lt;html&gt;<br>
<br>
&lt;head&gt;<br>
&lt;title&gt;Text der in Titelleiste erscheint&lt;/title&gt;<br>
&lt;/head&gt;<br>
<br>
&lt;body&gt;<br>
&lt;!-- Jetzt kommt der Text --&gt;<br>
<br>
&lt;h1&gt;Überschrift 1&lt;/h1&gt;<br>
<br>
<br>
&lt;hr&gt;<br>
<br>
Hier jetzt den Text eingeben:&lt;br&gt;<br>
&amp;lt;br&amp;gt; erzwingt Zeilenwechsel ohne<br>
Abstand zwischen Absätzen<br>
&amp;lt;p&amp;gt; erzwingt Absatzwechsel mit<br>
einer Zeile Abstand zum alten Absatz:<br>
&lt;p&gt;<br>
Text ohne Absatzschaltung &amp;lt;p&amp;gt; wird<br>
am Zeilenende automatisch umgebrochen.&lt;br&gt;<br>
&lt;b&gt;Fetter Text&lt;/b&gt;&lt;br&gt;<br>
&lt;i&gt;KursiverText&gt;&lt;br&gt;<br>
&lt;b&gt;&lt;i&gt;Fett und kursiv&lt;/i&gt;&lt;/b&gt;<br>
&lt;u&gt;Unterstrichener Text&lt;/u&gt;&lt;br&gt;<br>
&amp;quot;Anführungszeichen&amp;quot;<br>

<p>&lt;p&gt;<br>
&amp;auml;&lt;br&gt;<br>
&amp;ouml;&lt;br&gt;<br>
&amp;uuml;&lt;br&gt;<br>
&amp;szlig;&lt;br&gt;<br>
&lt;/p&gt;</p>

&lt;hr&gt;
<br>
<br>
&lt;/body&gt;<br>
&lt;/html&gt;

</td>

<td>

(markiert den Anfang eines html-Dokumentes)<br>
<br>
(Kopf-Anfang)<br>
...<br>
(Kopf-Ende)<br>
<br>
(markiert den Anfang des Textteiles)<br>
(so läßt sich ein Kommentar in den Quelltext einfügen)
<h1>Überschrift 1</h1>
<hr>
<p>Hier jetzt den Text eingeben:<br>
&lt;br&gt; erzwingt Zeilenwechsel ohne<br>
Abstand zwischen Absätzen<br>
&lt;p&gt; erzwingt Absatzwechsel mit<br>
einer Zeile Abstand zum alten Absatz:</p>
<br>
<p>Text ohne Absatzschaltung &lt;p&gt; wird<br>
am Zeilenende automatisch umgebrochen.<br>
<b>Fetter Text</b><br>
<i>KursiverText</i><br>
<b><i>Fett und kursiv</i></b><br>
<u>Unterstrichener Text</u><br>
&quot;Anführungszeichen&quot;<br>
</p>

<p>
&auml;<br>
&ouml;<br>
&uuml;<br>
&szlig;
</p>

<hr>
(Ende des Textes)<br>
(Ende des html-Dokumentes)

</td>
</tr>
</table>


# Springen innerhalb von Dokumenten

<table>
<tr>
<td>
&lt;a&nbsp;href=„#Vorwort“&gt;Vorwort&lt;/a&gt;
</td>
<td>
Link mit Text "Vorwort" (Verweis auf Sprungmarke "#Vorwort" markiert durch Rautensymbol; anklickbar)
</td>
</tr>
<tr>
<td>
...
</td>
<td>
</td>
</tr>
<tr>
<td>
&lt;a&nbsp;name=„Vorwort“&gt;Vorwort:&lt;/a&gt;
</td>
<td>
Vorwort: (Ziel des Verweises)
</td>
</tr>
</table>

# Springen zu externen Dokumenten

<table>
<tr>
<td>
&lt;a&nbsp;href=„http://www.tu-muenchen.de/homepage.htm“&gt;TUM&lt;/a&gt;
</td>
<td>
Link mit Text "TUM" auf die Homepage der TU München (externe Adresse im WWW)
</td>
</tr>

<tr>
<td colspan="2">
...
</td>
</tr>

<tr>
<td>
&lt;a&nbsp;href=„file://c:/htm/homepage.htm“&gt;Meine Homepage&lt;/a&gt;
</td>
<td>
Link mit Text "Meine Homepage" auf externe Adresse auf Festplatte
</td>
</tr>

<tr>
<td>
&lt;a&nbsp;href=„ftp://ftp.fu-berlin.de/pc/.../spiel.zip“&gt;Doom&lt;/a&gt;
</td>
<td>
Link mit Text "Doom" auf externe Adresse auf File-Server an UNI Berlin
</td>
</tr>
</table>

# Erstellen von Listen

## Punktliste (unsortierte Liste)

<table>
<tr>
<td>
&lt;ul&gt;<br>
&lt;li&gt;Hauptliste<br>
&nbsp;&nbsp;&lt;ul&gt;<br>
&nbsp;&nbsp;&lt;li&gt;Unterliste&lt;/li&gt;<br>
&nbsp;&nbsp;&lt;li&gt;bla bla ...&lt;/li&gt;<br>
&nbsp;&nbsp;&lt;/ul&gt;<br>
&lt;/li&gt;<br>
&lt;li&gt;Weiterer Hauptlistenpunkt&lt;/li&gt;<br>
&lt;/ul&gt;<br>
</td>
<td>
<ul>
<li>Hauptliste
  <ul>
  <li>Unterliste</li>
  <li>bla bla ...</li>
  </ul>
</li>
<li>Weiterer Hauptlistenpunkt</li>
</ul>
</td>
</tr>
</table>

## Nummerierte Liste (sortierte Liste)

<table>
<tr>
<td>
&lt;ol&gt;<br>
&lt;li&gt;Frühling<br>
&lt;li&gt;Sommer<br>
&nbsp;&nbsp;&lt;ol&gt;<br>
&nbsp;&nbsp;&lt;li&gt;Juli&lt;/li&gt;<br>
&nbsp;&nbsp;&lt;li&gt;August&lt;/li&gt;<br>
&nbsp;&nbsp;&lt;/ol&gt;<br>
&lt;/li&gt;<br>
&lt;li&gt;Herbst&lt;/li&gt;<br>
&lt;li&gt;Winter&lt;/li&gt;<br>
&lt;/ol&gt;<br>
</td>
<td>
<ol>
  <li>Frühling</li>
<li>Sommer
  <ol>
    <li>Juli</li>
    <li>August</li>
  </ol>
</li>
<li>Herbst</li>
<li>Winter</li>
</ol>
</td>
</tr>
</table>

## Definitionsliste

<table>
<tr>
<td>
&lt;ol&gt;<br>
&lt;li&gt;Frühling<br>
&lt;li&gt;Sommer<br>
&nbsp;&nbsp;&lt;ol&gt;<br>
&nbsp;&nbsp;&lt;li&gt;Juli&lt;/li&gt;<br>
&nbsp;&nbsp;&lt;li&gt;August&lt;/li&gt;<br>
&nbsp;&nbsp;&lt;/ol&gt;<br>
&lt;/li&gt;<br>
&lt;li&gt;Herbst&lt;/li&gt;<br>
&lt;li&gt;Winter&lt;/li&gt;<br>
&lt;/ol&gt;<br>
</td>
<td>
  
<dl>
<dt>Element 1
<dd>Text zu Element 1</dd>
</dt>
<dt>Element 2
<dd>Text zu Element 2</dd>
<dd>Noch ein Text zu Element 2</dd>
</dt>
</dl>
</td>
</tr>
</table>

# Tabellen

HTML-Quellcode:

```html
<table border="1">
  <tr>
    <th rowspan="2"></th>
    <th colspan="2">Durchschnitt</th>
    <th rowspan="2">sonstige Angaben</th>
  </tr>
  <tr>
    <th>Größe</th>
    <th>Gewicht</th>
  </tr>
  <tr>
    <th>männlich</th>
    <td>1.80</td>
    <td>85</td>
    <td>dick</td>
  </tr>
  <tr>
    <th>weiblich</th>
    <td>1.70</td>
    <td>65</td>
    <td>nett</td>
  </tr>
</table>
```

Ergebnis:

<table border="1">
  <tr>
    <th rowspan="2"></th>
    <th colspan="2">Durchschnitt</th>
    <th rowspan="2">sonstige Angaben</th>
  </tr>
  <tr>
    <th>Größe</th>
    <th>Gewicht</th>
  </tr>
  <tr>
    <th>männlich</th>
    <td>1.80</td>
    <td>85</td>
    <td>dick</td>
  </tr>
  <tr>
    <th>weiblich</th>
    <td>1.70</td>
    <td>65</td>
    <td>nett</td>
  </tr>
</table>

# Text als Adresse

HTML-Quellcode:

```html
<address>
<a href=„http://www.tu-muenchen.de/adressen/eichinger.html“>Ralf Eichinger</a>
</address>
```

Ergebnis:

<p>
<address>
<a href="http://www.tu-muenchen.de/adressen/eichinger.html">Ralf Eichinger</a>
</address>
</p>

# Vorformatierter Text

HTML-Quellcode:

```html
<pre>
Für Text, der genau so
umbrochen werden soll,
wie Sie ihn eingeben.
</pre>
```

Ergebnis:

<pre>
Für Text, der genau so
umbrochen werden soll,
wie Sie ihn eingeben.
</pre>

# Zitat

HTML-Quellcode:

```html
<blockquote>Zitat</blockquote>
```

Ergebnis:

<blockquote>Zitat</blockquote>


# Text mit nicht proportionalem Font

HTML-Quellcode:

```html
<tt>Text</tt>
```

Ergebnis:

<tt>Text</tt>


# Grafik einfügen

| &lt;img align="center" alt="Bild mit Landkarte von Brasilien" src="karte.gif"&gt; | Grafik „karte.gif“ wird zentriert eingefügt |

# Grafik als Verweis auf anderes Dokument

| &lt;a href="karte.htm"&gt;&lt;img src="karte.gif" align="center"&gt;&lt;/a&gt; | Bei Klick auf das Kartenbild efolgt Sprung zum „karte.htm“-Dokument |