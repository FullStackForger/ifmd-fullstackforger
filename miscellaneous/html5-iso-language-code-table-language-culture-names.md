# HTML5 – ISO LANGUAGE CODE TABLE – LANGUAGE CULTURE NAMES

Web Development might become real challenge when it comes to creating documents in languages other your native. I can cause quite a pain to both front- and back-end developers and administrators to make sure content is being displayed correctly.


## Language declaration

Language declaration is specification for full HTML document or chunk of HTML storing some content. Keep in mind it doesn’t provide any information on character encoding or  text direction (right to left or left to right), which have to be added additionally. Language declaration is often stated for:

- Language processing
- Text to speech converters (to localize language)
- Selecting the right fonts
- Selecting the spell-check dictionaries
- Language in HTML Document

HTML5 standard makes Content-Language obsolete, If used, W3C’s HTML5 validator will report error as follows: “Using the meta element to specify the document-wide default language is obsolete. Consider specifying the language on the root element instead.”.

Right now there are other three methods to declare the language.

### Pragma directive
As part of header in HTTP response, e.g. below:

```
HTTP/1.1 200 OK
Date: Wed, 05 Nov 2003 10:46:04 GMT
Server: Apache/1.3.28 (Unix) PHP/4.2.3
Content-Location: CSS2-REC.en.html
Vary: negotiate,accept-language,accept-charset
TCN: choice
P3P: policyref=http://www.w3.org/2001/05/P3P/p3p.xml
Cache-Control: max-age=21600
Expires: Wed, 05 Nov 2003 16:46:04 GMT
Last-Modified: Tue, 12 May 1998 22:18:49 GMT
ETag: "3558cac9;36f99e2b"
Accept-Ranges: bytes
Content-Length: 10734
Connection: close
Content-Type: text/html; charset=iso-8859-1
Content-Language: en
```

Example from W3C article on Internationalization Best Practices

### Lang attribute

It can be added to HTML element as lang  or a xml:lang attribute on XML documents used for example in SVG as shown below.
```html
<html lang="en">
```

It could be also use on other tags, which is useful especially for content displayed in two languages next to each other.
```html
<div lang="en"> ...English content...</div>
<div lang="de"> ...German content </div>
```

Few language codes could also be assign to one tag being comma separate
```
<div lang="en-us, en-uk, de"> ... multilingual content... </div>
```

## ISO Language Code Table / Language Culture
```
Code    Language (region)
af      Afrikaans
ar-ae   Arabic (U.A.E.)
ar-bh   Arabic (Bahrain)
ar-dz   Arabic (Algeria)
ar-eg   Arabic (Egypt)
ar-iq   Arabic (Iraq)
ar-jo   Arabic (Jordan)
ar-kw   Arabic (Kuwait)
ar-lb   Arabic (Lebanon)
ar-ly   Arabic (Libya)
ar-ma   Arabic (Morocco)
ar-om   Arabic (Oman)
ar-qa   Arabic (Qatar)
ar-sa   Arabic (Saudi Arabia)
ar-sy   Arabic (Syria)
ar-tn   Arabic (Tunisia)
ar-ye   Arabic (Yemen)
be      Belarusian
bg      Bulgarian
ca      Catalan
cs      Czech
da      Danish
de      German (Standard)
de-at   German (Austria)
de-ch   German (Switzerland)
de-li   German (Liechtenstein)
de-lu   German (Luxembourg)
el      Greek
en      English
en-au   English (Australia)
en-bz   English (Belize)
en-ca   English (Canada)
en-gb   English (United Kingdom)
en-ie   English (Ireland)
en-jm   English (Jamaica)
en-nz   English (New Zealand)
en-tt   English (Trinidad)
en-us   English (United States)
en-za   English (South Africa)
es      Spanish (Spain)
es-ar   Spanish (Argentina)
es-bo   Spanish (Bolivia)
es-cl   Spanish (Chile)
es-co   Spanish (Colombia)
es-cr   Spanish (Costa Rica)
es-do   Spanish (Dominican Republic)
es-ec   Spanish (Ecuador)
es-gt   Spanish (Guatemala)
es-hn   Spanish (Honduras)
es-mx   Spanish (Mexico)
es-ni   Spanish (Nicaragua)
es-pa   Spanish (Panama)
es-pe   Spanish (Peru)
es-pr   Spanish (Puerto Rico)
es-py   Spanish (Paraguay)
es-sv   Spanish (El Salvador)
es-uy   Spanish (Uruguay)
es-ve   Spanish (Venezuela)
et      Estonian
eu      Basque
fa      Farsi
fi      Finnish
fo      Faeroese
fr      French (Standard)
fr-be   French (Belgium)
fr-ca   French (Canada)
fr-ch   French (Switzerland)
fr-lu   French (Luxembourg)
ga      Irish
gd      Gaelic (Scotland)
he      Hebrew
hi      Hindi
hr      Croatian
hu      Hungarian
id      Indonesian
is      Icelandic
it      Italian (Standard)
it-ch   Italian (Switzerland)
ja      Japanese
ji      Yiddish
ko      Korean
ko      Korean (Johab)
ku      Kurdish
lt      Lithuanian
lv      Latvian
mk      Macedonian (FYROM)
ml      Malayalam
ms      Malaysian
mt      Maltese
nl      Dutch (Standard)
nl-be   Dutch (Belgium)
nb      Norwegian (Bokmål)
nn      Norwegian (Nynorsk)
no      Norwegian
pa      Punjabi
pl      Polish
pt      Portuguese (Portugal)
pt-br   Portuguese (Brazil)
rm      Rhaeto-Romanic
ro      Romanian
ro-md   Romanian (Republic of Moldova)
ru      Russian
ru-md   Russian (Republic of Moldova)
sb      Sorbian
sk      Slovak
sl      Slovenian
sq      Albanian
sr      Serbian
sv      Swedish
sv-fi   Swedish (Finland)
th      Thai
tn      Tswana
tr      Turkish
ts      Tsonga
uk      Ukrainian
ur      Urdu
ve      Venda
vi      Vietnamese
xh      Xhosa
zh-cn   Chinese (PRC)
zh-hk   Chinese (Hong Kong)
zh-sg   Chinese (Singapore)
zh-tw   Chinese (Taiwan)
zu      Zulu
```
