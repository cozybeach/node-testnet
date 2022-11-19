# **ALEO CLIENT & PROVER NODE**

<p align="center">
  <img width="460" height="300" src="https://pbs.twimg.com/media/FhKBJarXoAIaFnA?format=jpg&name=large">
</p>

## **Hardware Specifications**

| Component  | Reommendation |
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

## **2. Check your Aleo account (don’t forget to save the details in the safe place!)**

```
cat $HOME/aleo/account_new.txt
```

## **3. Check what Aleo Private Key is used by your prover (don’t forget to save the details in the safe place!)**

```
grep "prover" /etc/systemd/system/aleo-prover.service | awk '{print $5}'
```

## **4. Check aleo prover logs**

```
journalctl -u aleo-prover -f -o cat
```

## **5. Check the aleo client logs if it is running**

```
journalctl -u aleo-client -f -o cat
```



> **If you've completed all of these steps, your prover node is up and running**. And **_you can skip using the USEFUL COMMANDS down below_**. **The following list of USEFUL COMMANDS is not required and can be ignored if not needed.**

## Stop the aleo prover and start the aleo client

```
systemctl stop aleo-prover
systemctl restart aleo-client
```

## Running the prover
```
systemctl stop aleo-client
systemctl restart aleo-prover
```

## Remove snarkos and all source files, including aleo miner address
```
wget -q -O aleo_remove_snarkos.sh https://api.nodes.guru/aleo_remove_snarkos2.sh && chmod +x aleo_remove_snarkos.sh && sudo /bin/bash aleo_remove_snarkos.sh
```
