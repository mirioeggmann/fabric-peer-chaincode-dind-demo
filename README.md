# Fabric-Peer Chaincode DinD Demo

Built with the fabric-samples [basic-network](https://github.com/hyperledger/fabric-samples/tree/release-1.1/basic-network).
Demo is realized with docker-compose and not swarm, because there is currently no priviledged=true option for containers in swarm.

Commands:
- peer chaincode install -n mycc -p github.com/chaincode_example02/go/ -v v0.1
- peer chaincode instantiate -o orderer.example.com:7050 -C mychannel -n mycc -v v0.1 -c '{"Args":["init","a","100","b","200"]}'
- peer chaincode invoke -o orderer.example.com:7050 -C mychannel -n mycc -c '{"Args":["invoke","a","b","10"]}'
- peer chaincode query -C mychannel -n mycc -c '{"Args":["query","a"]}'
