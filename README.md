# Auditor API
Queries a county REST API, parses JSON response, and applies an auditor-MISMO JSON map to label the data.

## About
The Mortgage Industry Standards Maintenance Organization (MISMO) provides an XML data structure as a common language to exchange data across the mortgage finance industry. The Appraise-It program, to which our ultimate output is, happens to have a MISMO XML import function.

The county auditor makes available data, which we need inputted into Appraise-It, via RESTful API. The API outputs a JSON data structure. Because the county data structure and the mortgage industry's XML are different, I've mapped their fields one-to-one.

Both XML and JSON can be thought of as data dictionaries with key-value pairs, conceptually: key=value, eg parcel=1234567 or owner='John Oberlin'. An XML structure is constructed of elements, within which optional attributes are embedded. Attributes are like keys, as they have values. A JSON structure can be thought of as sets of keys and values.

So, in the MISMO XML, a parcel with number 1234567 looks like:

```
<PARCEL_IDENTIFIER GSEAssessorsParcelIdentifier="1234567" />
```

And in the county ParcelData API ouput in JSON:

```
"PARCEL_NUMBER": "1234567"
```

My mapping dictionary structure (happens to be in JSON as well, because it looks cleaner than XML):

```
"API": {
	"MISMO_XML_ELEMENT": {
		"MISMO_ElementAttribute": "API_FIELD"
	}
}
```

That is to say that, the API_FIELD maps directly to the MISMO_ElementAttribute, whose parent is a MISMO_XML_ELEMENT. (This retains the element-attribute relationship from the XML.) All of which are under a related API.

Here's a slice from the schema concerning parcel number:

```
"ParcelData" {
	"PARCEL_IDENTIFIER": {
		"GSEAssessorsParcelIdentifier": "PARCEL_NUMBER"
	}
}
```

So now when, retrieving API data and hoping to output it as MISMO formatted XML, we can programmatially say, 'We're getting PARCEL_NUMBER from the API, so its value should now be the value for GSEAssessorsParcelIdentifier within PARCEL_IDENTIFIER.'

## Links
 * [API-MISMO Mapping in JSON](https://github.com/oberljn/auditor-api/blob/master/mismo_map.json)
 * [MISMO Standards](http://www.mismo.org/get-started/adopt-the-standards)
 * [Stark County Open Data](http://opendata.starkcountyohio.gov)

