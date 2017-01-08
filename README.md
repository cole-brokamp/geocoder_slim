# geocoder_slim

> This is a slimmed down and Dockerized version of the offline geocoder here: https://github.com/cole-brokamp/geocoder.

#### Install

The image is hosted privately and can be pulled with `docker pull 126952269818.dkr.ecr.us-east-1.amazonaws.com/geocoder_slim`. This requires authorization through the amazon web services command line tools for the EC2
container registry service. Contact the author for access. Note that this could take a while because the image contains a large (~5GB) database of TIGER/Line address range files.

#### Single Geocode

Call the container and geocode a single address. Delete the container afterwards:

```
docker run --rm=true 126952269818.dkr.ecr.us-east-1.amazonaws.com/geocoder_slim bash -c "ruby /root/geocoder/geocode.rb '3333 Burnet Ave Cincinnati OH 45229'"
```

The software will return the geocoding result as JSON to stdout:

```
[{"street":"Burnet Ave","zip":"45229","city":"Cincinnati","state":"OH","lat":39.14089,"lon":-84.500402,"fips_county":"39061","score":0.949,"prenum":"","number":"3333","precision":"range"}]
```

See the original software package (https://bitbucket.org/cole-brokamp/geocoder) for documentation and
interpretation of output.

#### Continuous Geocoding

Start the container and keep it running with: `docker run -it -d --name gs 126952269818.dkr.ecr.us-east-1.amazonaws.com/geocoder_slim /bin/bash`

Query the container for a geocode with `docker exec gs ruby /root/geocoder/geocode.rb '3333 Burnet Ave Cincinnati OH 45229'`

Continue querying the container with more addresses. This can be automated for several addresses using a scripting language.

Stop the container and delete it with `docker stop gs` and `docker rm gs`.
