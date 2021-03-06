# Install CLI
```
wget https://github.com/kubeless/kubeless/releases/download/v1.0.0-alpha.8/kubeless_linux-amd64.zip
unzip kubeless_linux-amd64.zip
sudo mv bundles/kubeless_linux-amd64/kubeless /usr/local/bin
rm -r bundles/
```

# Role Binding
kubectl create clusterrolebinding cluster-admin-binding --clusterrole=cluster-admin  --user=$(gcloud config get-value core/account)

# Deploy kubeless
kubectl create ns kubeless

kubectl create -f https://github.com/kubeless/kubeless/releases/download/v1.0.0-alpha.8/kubeless-v1.0.0-alpha.8.yaml 

# Example function

## python
```
kubeless function deploy hello --runtime python2.7 \
                               --from-file python-example/example.py \
                               --handler test.hello
```
## NodeJS
```
kubeless function deploy myfunction --runtime nodejs6 \
                                --dependencies node-example/package.json \
                                --handler test.myfunction \
                                --from-file node-example/example.js
```

# Commands

## List Function
```
kubeless function ls
```
## Call Function
```
kubeless function call myfunction --data 'This is some data'
```

## Expose function
```
# Require Extrnal DNS Resolution
kubectl create -f nginx-ingress-controller-with-elb.yml
kubeless trigger http create myfunction --function-name myfunction --hostname myfunction.kubernetes.cluster.local

# WorkAround
# Expose the myfuction service on nodetype LoadBalancer or NodePort
```



# PubSub
## Kafka Installation
```
export RELEASE=$(curl -s https://api.github.com/repos/kubeless/kafka-trigger/releases/latest | grep tag_name | cut -d '"' -f 4)
kubectl create -f https://github.com/kubeless/kafka-trigger/releases/download/$RELEASE/kafka-zookeeper-$RELEASE.yaml
```

## Deploy function
```
kubeless function deploy uppercase --runtime nodejs6 \
                                --dependencies node-example/package.json \
                                --handler test.uppercase \
                                --from-file node-example/uppercase.js
```

## Trigger and publish
```
kubeless trigger kafka create test --function-selector created-by=kubeless,function=uppercase --trigger-topic uppercase
kubeless topic publish --topic uppercase --data "this message will be converted to uppercase"
```
