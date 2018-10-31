# Fabric-Peer Chaincode DinD Demo

Built with the fabric-samples [basic-network](https://github.com/hyperledger/fabric-samples/tree/release-1.1/basic-network).
Demo is realized with docker-compose and not swarm, because there is currently no priviledged=true option for containers in swarm.

## Commands

```yaml
peer chaincode install -n mycc -p github.com/chaincode_example02/go/ -v v0.1
peer chaincode instantiate -o orderer.example.com:7050 -C mychannel -n mycc -v v0.1 -c '{"Args":["init","a","100","b","200"]}'
peer chaincode invoke -o orderer.example.com:7050 -C mychannel -n mycc -c '{"Args":["invoke","a","b","10"]}'
peer chaincode query -C mychannel -n mycc -c '{"Args":["query","a"]}'
```

## peer0 without dind sidecar


## peer0 with dind sidecar
```yaml
peer0.org1.example.com:
    container_name: peer0.org1.example.com
    image: hyperledger/fabric-peer:x86_64-1.1.0
    environment:
      - CORE_VM_ENDPOINT=tcp://dind-peer0:2375
      - CORE_PEER_ID=peer0.org1.example.com
      - CORE_LOGGING_PEER=debug
      - CORE_CHAINCODE_LOGGING_LEVEL=DEBUG
      - CORE_PEER_LOCALMSPID=Org1MSP
      - CORE_PEER_MSPCONFIGPATH=/etc/hyperledger/msp/peer/
      - CORE_PEER_ADDRESS=peer0.org1.example.com:7051
      - CORE_LEDGER_STATE_STATEDATABASE=CouchDB
      - CORE_LEDGER_STATE_COUCHDBCONFIG_COUCHDBADDRESS=couchdb:5984
      - CORE_LEDGER_STATE_COUCHDBCONFIG_USERNAME=
      - CORE_LEDGER_STATE_COUCHDBCONFIG_PASSWORD=
    working_dir: /opt/gopath/src/github.com/hyperledger/fabric
    command: peer node start
    ports:
      - 7051:7051
      - 7053:7053
    volumes:
        - ./data/crypto-config/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/msp:/etc/hyperledger/msp/peer
        - ./data/crypto-config/peerOrganizations/org1.example.com/users:/etc/hyperledger/msp/users
        - ./data/config:/etc/hyperledger/configtx
    depends_on:
      - orderer.example.com
      - couchdb
    links:
      - dind-peer0
```

## dind-sidecar container
```yaml
dind-peer0:
    hostname: dind-peer0
    image: docker:dind
    privileged: true
    ports:
      - '2375:2375'
      - '2376:2376'
```

