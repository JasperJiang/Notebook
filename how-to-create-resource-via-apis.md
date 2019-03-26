# How to create resource via APIs

{% api-method method="post" host="{{hostname}}" path="agent-service/api/createResource" %}
{% api-method-summary %}
createResource
{% endapi-method-summary %}

{% api-method-description %}
This endpoint allows you to get free cakes.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-headers %}
{% api-method-parameter name="idamToken" type="string" required=false %}
IdAM Token
{% endapi-method-parameter %}

{% api-method-parameter name="idamAccessToken" type="string" required=false %}
IdAM AccessToken
{% endapi-method-parameter %}

{% api-method-parameter name="apiKey" type="string" required=false %}
API Key
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-form-data-parameters %}
{% api-method-parameter name="componentType" type="array" required=true %}

{% endapi-method-parameter %}

{% api-method-parameter name="resConfig" type="object" required=true %}

{% endapi-method-parameter %}

{% api-method-parameter name="resourceDescription" type="string" required=false %}

{% endapi-method-parameter %}

{% api-method-parameter name="resourceName" type="string" required=true %}

{% endapi-method-parameter %}

{% api-method-parameter name="resourceType" type="string" required=true %}

{% endapi-method-parameter %}

{% api-method-parameter name="file" type="object" required=true %}

{% endapi-method-parameter %}
{% endapi-method-form-data-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
Cake successfully retrieved.
{% endapi-method-response-example-description %}

```javascript
{
    "id": 17894,
    "name": "testPython",
    "physicalName": "python2_test_0426.py",
    "description": "this is description",
    "isZip": false
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

**resourceType selections:**

| Selections | Description |
| :--- | :--- |
| SPARK\_ONE\_DOT\_SIX\_PYTHON\_2 | Spark 1.6 + Python 2.x |
| SPARK\_TWO\_PYTHON\_2 | Spark 2.0 + Python 2.x |
| SPARK\_TWO\_PYTHON\_3 | Spark 2.0 + Python 3.x |
| SPARK\_ONE\_DOT\_SIX\_R | Spark 1.6 + R |
| SPARK\_TWO\_R | Spark 2.0 + R |
| SPARK\_ONE\_DOT\_SIX\_SCALA | Spark 1.6 + Scala/Java |
| SPARK\_TWO\_SCALA | Spark 2.0 + Scala/Java |
| PYTHON2 | Python 2.7x |
| PYTHON3 | Python 3.6x |
| R | R 3.1x |
| PENTAHO | Pentaho |
| RULES | Rules |

**ComponentType Selections:**

| **selections** |
| :--- |
| Data\_Ingestion |
| Data\_Preparation |
|  Model |
| Post\_Model\_Data\_Preparation |

**resConfig examples:**

\*\*\*\*

<table>
  <thead>
    <tr>
      <th style="text-align:left">Resource Type</th>
      <th style="text-align:left">Config Example</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">Spark</td>
      <td style="text-align:left">
        <p>{</p>
        <p>&quot;mainClass&quot;: &quot;mainClass&quot;,</p>
        <p>&quot;sparkConf&quot;: {</p>
        <p>&quot;sck1&quot;: &quot;scv1&quot;</p>
        <p>},</p>
        <p>&quot;packages&quot;: [&quot;p1&quot;,&quot;p2&quot;],</p>
        <p>&quot;supPyRes&quot;: [&quot;sp1&quot;,&quot;sp2&quot;],</p>
        <p>&quot;supJars&quot;: [&quot;sj1&quot;,&quot;sj2&quot;],</p>
        <p>&quot;appArgs&quot;: [&quot;a1&quot;,&quot;pa2&quot;],</p>
        <p>&quot;sparkArgs&quot;: {</p>
        <p>&quot;sak1&quot;: &quot;sav1&quot;</p>
        <p>}</p>
        <p>}</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Rules</td>
      <td style="text-align:left">
        <p>{</p>
        <p>&quot;params&quot;:{</p>
        <p>&quot;pk1&quot;: &quot;pv1&quot;,</p>
        <p>&quot;pk2&quot;: &quot;pv2&quot;</p>
        <p>}</p>
        <p>}</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">R</td>
      <td style="text-align:left">
        <p>{</p>
        <p>&quot;packages&quot;: [&quot;p1&quot;,&quot;p2&quot;],</p>
        <p>&quot;supRes&quot;: [&quot;s1&quot;, &quot;s2&quot;],</p>
        <p>&quot;appArgs&quot;: [&quot;a1&quot;, &quot;a2&quot;]</p>
        <p>}</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Python</td>
      <td style="text-align:left">
        <p>{</p>
        <p>&quot;packages&quot;: [&quot;p1&quot;,&quot;p2&quot;],</p>
        <p>&quot;supRes&quot;: [&quot;s1&quot;, &quot;s2&quot;],</p>
        <p>&quot;appArgs&quot;: [&quot;a1&quot;, &quot;a2&quot;]</p>
        <p>}</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Pentaho</td>
      <td style="text-align:left">
        <p>{</p>
        <p>&quot;params&quot;:{</p>
        <p>&quot;pk1&quot;: &quot;pv1&quot;,</p>
        <p>&quot;pk2&quot;: &quot;pv2&quot;</p>
        <p>}</p>
        <p>}</p>
      </td>
    </tr>
  </tbody>
</table>