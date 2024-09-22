# Themis
This repository contains the source codes of the prototype for our submission to SOSP'24 poster: `Themis: Efficiently Mitigating Congestion-Induced Fairness Disparities in Long-Haul RDMA Networks`.
Here are edited files:
- `SwitchNode::SwitchNotifyDequeue`: if it's a External Switch, after get packets with ECN mark, switches will proactively send CNP to the target sender at intervals of 10us, similar to the Nofitifcation Point on the receiver of DCQCN.
- `QbbNetDevice::ReceiveCnp`: records info in `m_cnp_hanlder` if it receives a CNP from hosts.
- `QbbNetDevice::DequeueAndTransmit`: External Switch will act as a early Reaction Point, resubmit operated in this function.
# Evaluation
Our simulator is based on the [RDMA simulator for HPCC](https://github.com/alibaba-edu/High-Precision-Congestion-Control). It is based on NS-3 version 3.17.
## Build
```
cd simulation
sudo chmod +x waf
./waf configure
```
**Please note if gcc version > 5, compilation will fail due to some ns3 code style. If this what you encounter, please use:** `CC='gcc-5' CXX='g++-5' ./waf configure`
## Run
At directory of `simulation`
```
./waf --run 'scratch/third mix/config.txt'
cd ./mix
python3 lrx_analyze.py
```
- In `./mix/config.txt`
- Set `ENABLE_THEMIS` to true, test our solution
- Set `ENABLE_THEMIS` to false, and set `CC_MODE` to 1, test origin DCQCN solution
- Set `ENABLE_THEMIS` to false, and set `CC_MODE` to 3, test origin DCQCN solution