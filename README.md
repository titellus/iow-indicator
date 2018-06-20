
From indicators collected in various regions and stored in Postgresql, load them into Elasticsearch and use Kibana to create custom dashboards.

# Installation

Download Elasticsearch from https://www.elastic.co/fr/downloads/elasticsearch
Unzip Elasticsearch and start it with ```elasticsearch.bat```.
Once started, create the ```indicator``` index:

```
curl -X DELETE http://localhost:9200/iowindicators 
curl -X PUT http://localhost:9200/iowindicators -H "Content-type:application/json" -d @iowindicators_index.json
```


Download Kibana from https://www.elastic.co/fr/downloads/kibana
Configure Kibana in ```config/kibana.yaml``` and add the base data layers.

```
regionmap:
  layers:
     - name: "Administrative regions - R0 (IOW)"
       url: "https://files.titellus.net/t_adreg_r0.geojson"
       fields:
          - name: "adrgcode"
            description: "adrgcode"
     - name: "Administrative regions - R1 (IOW)"
       url: "https://files.titellus.net/t_adreg_r1.geojson"
       fields:
          - name: "adrgcode"
            description: "adrgcode"
     - name: "Administrative regions - R2 (IOW)"
       url: "https://files.titellus.net/t_adreg_r2.geojson"
       fields:
          - name: "adrgcode"
            description: "adrgcode"
```

# Loading indicators in the index

Run talend job ```indicator2index```.



# Creating base data layers

From QGIS, load the ```t_adreg``` table with level filter if needed and save file as GeoJSON in WGS84 projection. Limit the number of decimal to ```2``` to avoid to create too big files.
