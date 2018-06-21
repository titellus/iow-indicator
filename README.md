
From indicators collected in various regions and stored in Postgresql, load them into Elasticsearch and use Kibana to create custom dashboards.

# Installation

Download Elasticsearch from https://www.elastic.co/fr/downloads/elasticsearch
Unzip Elasticsearch and start it with ```elasticsearch.bat```.
Once started, create the ```iowindicators``` index:

```
curl -X DELETE http://localhost:9200/iowindicators 
curl -X PUT http://localhost:9200/iowindicators -H "Content-type:application/json" -d @iowindicators_index.json
```

Check that the index is created by browsing to http://localhost:9200/iowindicators.


Download Kibana from https://www.elastic.co/fr/downloads/kibana
Configure Kibana in ```config/kibana.yaml``` and add:

* server base path to indicate the path the proxy is using to access kibana
* the base data layers.

```
server.basePath: "/dashboards"

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
     - name: "River basins - B0 (IOW)"
       url: "https://files.titellus.net/t_hybasin_b0.geojson"
       fields:
          - name: "hybcode"
            description: "hybcode"
     - name: "River basins - B1 (IOW)"
       url: "https://files.titellus.net/t_hybasin_b1.geojson"
       fields:
          - name: "hybcode"
            description: "hybcode"
     - name: "River basins - B2 (IOW)"
       url: "https://files.titellus.net/t_hybasin_b2.geojson"
       fields:
          - name: "hybcode"
            description: "hybcode"

```

On the Apache webserver, configure the proxy pass to Kibana

```
  ProxyPass /dashboards http://localhost:5601
  <Location /dashboards>
    ProxyPassReverse http://localhost:5601
  </Location>
```



# Loading indicators in the index

Run talend job ```indicator2index``` (see ```resources/talendjob.zip```).



# Creating base data layers

From QGIS, load the ```t_adreg``` table with level filter if needed and save file as GeoJSON in WGS84 projection. Limit the number of decimal to ```2``` to avoid to create too big files (see ```resources``` folder for files).
