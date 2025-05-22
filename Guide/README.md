## Steps Taken

### 1. Initial OS Setup
- Installed Raspberry Pi OS using Raspberry Pi Imager.
- Connected to the internet via Ethernet.
- Performed system update:
  
  ```bash
  sudo apt update && sudo apt upgrade -y
  ````
  ---
  
 **2. Mosquitto MQTT Installation Attempt (Failed)**

 
- Tried installing Mosquitto:

````bash
sudo apt install -y mosquitto mosquitto-clients
````
- **Error:**

````vbnet
E: Release 'clients' for mosquitto was not found
````
- **Cause**: Mosquitto was removed from Raspberry Pi OS Bookworm's default repo.

---

**3. Fix Attempt with PPA (Failed)**

- Tried adding the mosquitto-dev PPA:

````bash
sudo add-apt-repository ppa:mosquitto-dev/mosquitto-ppa
````
- **Result**: Incompatible with Bookworm.
---

**4. Final Working Solution**

Added Mosquitto official repo:

````bash
sudo apt install -y curl gnupg
curl -fsSL https://repo.mosquitto.org/debian/mosquitto-repo.gpg.key | sudo tee /usr/share/keyrings/mosquitto-archive-keyring.gpg > /dev/null

echo "deb [signed-by=/usr/share/keyrings/mosquitto-archive-keyring.gpg] http://repo.mosquitto.org/debian bookworm main" | sudo tee /etc/apt/sources.list.d/mosquitto.list > /dev/null
Updated and installed:
````

````bash
sudo apt update
sudo apt install -y mosquitto mosquitto-clients
Enabled and started Mosquitto:
````

````bash
sudo systemctl enable mosquitto
sudo systemctl start mosquitto
````
---

**5. MQTT Broker Testing**

- Verified via two terminals:

- 1. **Terminal 1**
````bash
mosquitto_sub -t "test/topic"
````
- 2. **Terminal 2**

````bash    
mosquitto_pub -t "test/topic" -m "Hello"
````
---

**6. Arduino Uno WiFi Rev2 Integration**

- Arduino connects to WiFi and MQTT server (Raspberry Pi)

- Reads LDR values and publishes formatted message:

On Raspberry Pi:

````bash
mosquitto_sub -t "solartracker/ldr"
````
--- 

Output:

`Top: 735 | Bottom: 689 | Left: 702 | Right: 710`
