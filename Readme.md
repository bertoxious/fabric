```shell
./network.up up createChannel -c first
```

command to install java chaincode

```shell
./network.sh deployCC -c first -ccn basic -ccp ../asset-transfer-basic/chaincode-java -ccl java
```

Set these env variables
```shell
cd fabric-samples/test-network 
nano env.sh
```

```shell
export FABRIC_CFG_PATH=${PWD}/../config/
export CORE_PEER_LOCALMSPID="Org1MSP"
export CORE_PEER_TLS_ENABLED=true
export CORE_PEER_TLS_ROOTCERT_FILE=${PWD}/organizations/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt
export CORE_PEER_MSPCONFIGPATH=${PWD}/organizations/peerOrganizations/org1.example.com/users/Admin@org1.example.com/msp

export CORE_PEER_ADDRESS=localhost:7051
```

```shell
source env.sh
```
query smart contracts to query all of the assets present in the ledger

```shell
../bin/peer chaincode query -C first -n basic -c '{"Args":["GetAllAssets"]}' | jq
```

```shell
cd test-network
../bin/peer chaincode invoke -o localhost:7050 --ordererTLSHostnameOverride orderer.example.com --tls --cafile "${PWD}/organizations/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem" -C first -n basic --peerAddresses localhost:7051 --tlsRootCertFiles "${PWD}/organizations/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt" --peerAddresses localhost:9051 --tlsRootCertFiles "${PWD}/organizations/peerOrganizations/org2.example.com/peers/peer0.org2.example.com/tls/ca.crt" -c '{"function": "InitLedger","Args": []}'
```
data-request-chaincode-installation
```shell
./network.sh deployCC -c first -ccn data-request -ccp ../asset-transfer-basic/chaincode-data-request -ccl java
```
data-request init ledger
```shell
 ../bin/peer chaincode invoke -o localhost:7050 --ordererTLSHostnameOverride orderer.example.com --tls --cafile "${PWD}/organizations/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem" -C first -n data-request --peerAddresses localhost:7051 --tlsRootCertFiles "${PWD}/organizations/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt" --peerAddresses localhost:9051 --tlsRootCertFiles "${PWD}/organizations/peerOrganizations/org2.example.com/peers/peer0.org2.example.com/tls/ca.crt" -c '{"function": "InitDataRequestLedger","Args": []}'
```
get-all-data-requests
```shell
../bin/peer chaincode query -C first -n data-request -c '{"Args":["GetAllDataRequests"]}' | jq
```