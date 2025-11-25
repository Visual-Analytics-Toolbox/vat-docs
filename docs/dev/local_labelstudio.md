# Labelstudio Dev Setup

http://host.docker.internal:9090/

docker run -it -p 9001:8080 -v $(pwd)/mydata:/label-studio/data heartexlabs/label-studio:latest label-studio

webhook url:  http://host.docker.internal:8000/image/validate