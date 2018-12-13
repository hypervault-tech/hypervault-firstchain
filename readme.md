# HyperVault FirstChain

This is a stripped-down toy model that I have been playing with to learn more about hyperledger. This is based on the `release-1.4` version of [fabric-samples](https://github.com/hyperledger/fabric-samples/releases/tag/v1.4.0-rc1)

## Starting the `basic-network`

The basic network consists of just one organisation, one orderer and one peer. 

To start the network on the hypervault.tech server, run 

```sudo ./start.sh```

 in the `basic-network` directory. This brings up the network that consists of the orderer and the peer. The script additionally initiates a channel called `mychannel`, which stands for HyperVaultChannel, and joins the peer to the channel. 

## Installing and instantiating the hypervault chaincode

To install and instantiate the chaincode, we need to bring up the `cli` container to act as the admin. 

Run 

```sudo docker-compose -f docker-compose.yml up -d cli``` 

to bring up the container and then go into the container by 

```sudo docker exec -it cli bash``` 

Voila you are now in the `cli` container, ready to take control of the network. 

To check the channels that this entity is part of, simply run `peer channel list`

To install the chaincode, simply run in the `cli` container

```peer chaincode install -l node -n hypervault -v 0 -p /opt/gopath/src/github.com/hypervault/lowlevel```

To verify that the chaincode has been successfully installed, run 

```peer chaincode list -C mychannel --installed```

Finally, instantiate the chaincode by running 

```peer chaincode instantiate -n hypervault -v 0 -l node -C mychannel -c '{"Args":[]}'```

The `-c` flag is required even though we do not need any arguments in the `Args` field. 

## Invoking the chaincode

