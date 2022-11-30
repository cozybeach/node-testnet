# **ALEO CLIENT & PROVER NODE**

<p align="center">
  <img width="460" height="300" src="https://pbs.twimg.com/media/FhKBJarXoAIaFnA?format=jpg&name=large">
</p>

## **Hardware Specifications**

| Component  | Recommendation |
| ------------- | ------------- |
| CPU  | 16 CPU **(32 CPU preferred)**  |
| Memory  | 16gb RAM **(32gb RAM preferred)** |
| Storage  | 128gb disk space |
| Network | 10 Mbps of upload and download bandwidth |
| Type | **Dedicated Server** |

# **HOW TO RUN ALEO CLIENT & PROVER NODE**

## 1. **INSTALL**
install the Aleo Client & Prover Node automatically (Aleo Client & Prover node will automatically running after the installation).

```
wget -q -O aleo_snarkos3.sh https://api.nodes.guru/aleo_snarkos3.sh && chmod +x aleo_snarkos3.sh && sudo /bin/bash aleo_snarkos3.sh
```

## **2. CHECK YOUR ALEO ACCOUNT (SAVE THE DETAILS IN THE SAFE PLACE!)**

```
cat $HOME/aleo/account_new.txt
```

## **3. CHECK WHAT ALEO PRIVATE KEY IS USED BY YOUR PROVER (SAVE THE DETAILS IN THE SAFE PLACE!)**

```
grep "prover" /etc/systemd/system/aleo-prover.service | awk '{print $5}'
```

## **4. CHECK ALEO PROVER LOGS**

```
journalctl -u aleo-prover -f -o cat
```

## **5. CHECK THE ALEO CLIENT LOGS IF IT IS RUNNINGg**

```
journalctl -u aleo-client -f -o cat
```



> **If you've completed all of these steps, your prover node is up and running**. And **_you can skip using the USEFUL COMMANDS down below_**. **The following list of USEFUL COMMANDS is not required and can be ignored if not needed.**

## STOP THE ALEO PROVER AND START THE ALEO CLIENT

```
systemctl stop aleo-prover
systemctl restart aleo-client
```

## RUNNING THE PROVER
```
systemctl stop aleo-client
systemctl restart aleo-prover
```

## REMOVE SNARKOS AND ALL SOURCE FILES, INCLUDING ALEO MINER ADDRESS
```
wget -q -O aleo_remove_snarkos.sh https://api.nodes.guru/aleo_remove_snarkos2.sh && chmod +x aleo_remove_snarkos.sh && sudo /bin/bash aleo_remove_snarkos.sh
```

