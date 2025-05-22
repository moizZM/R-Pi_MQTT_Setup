# RP132-

curl -fsSL https://repo.mosquitto.org/debian/mosquitto-repo.gpg.key | sudo tee /usr/share/keyrings/mosquitto-archive-keyring.gpg > /dev/null

echo "deb [signed-by=/usr/share/keyrings/mosquitto-archive-keyring.gpg] http://repo.mosquitto.org/debian bookworm main" | sudo tee /etc/apt/sources.list.d/mosquitto.list > /dev/null

