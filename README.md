# ESPHome BLE xiaomi teploměr LYWSD03MMC

## Popis:
Postup pro preflashování BLE teploměru Xiaomi LYWSD03MMC na opensource firmware ATC, který je kompatibilní s ESPHome.
Zařízení potom komunikuje přímo s ESP32 bez prostředníka a lze v programu dále pracovat s entitami teploměru.
Z ESP32 se stane bluetooth proxy (brána) přes WIFI. Lze připojovat více BLE zařízení.
<img src="https://github.com/peca2345/ESPHome-BLE-xiaomi-LYWSD03MMC/blob/main/IMG/xiaomi_ble_temperature.jpg?raw=true" alt="IMG" width="400" height="300">


![IMG](https://github.com/peca2345/ESPHome-BLE-xiaomi-LYWSD03MMC/blob/main/IMG/xiaomi_ble_temperature.jpg?raw=true)

## Info:
- dosah cca 20m
- možnost nastavení intervalu odesílání dat 
- vydrž baterie dle výrobce 1-2 roky při intervalu odesílání dat každých 5 minut
- možnost vypnutí smajlu na LCD
- signál se mezi zařízeními nepřeposíla jako u zigbee.
- na jednu instanci Home Assistanta můžete oproti zigbee použít více BLE proxy bran pro větší pokrytí 



## Komponenty:
- Xiaomi BLE senzor: [SHOP](https://www.aliexpress.com/item/1005004038986541.html?spm=a2g0o.productlist.main.19.6c0828ab2pLzZe&algo_pvid=b4e00c04-2583-4e5c-83e7-fa112be91db1&algo_exp_id=b4e00c04-2583-4e5c-83e7-fa112be91db1-9&pdp_npi=3%40dis%21CZK%21199.45%21137.54%21%21%21%21%21%40211bf04a16827115197988812d07a0%2112000027838365642%21sea%21CZ%21166466096&curPageLogUid=mf5jOVf44hjo)
- ESP32: [SHOP](https://www.aliexpress.com/item/1005005246146177.html?spm=a2g0o.productlist.main.1.18cd7404DIFcOK&algo_pvid=daedd95e-ba9a-4aed-a695-54dff0fa1af4&algo_exp_id=daedd95e-ba9a-4aed-a695-54dff0fa1af4-0&pdp_npi=3%40dis%21CZK%21118.58%2181.74%21%21%21%21%21%40211bf4c516827119032341282d07ad%2112000032346510820%21sea%21CZ%21166466096&curPageLogUid=NmCOeBunLWwI)

## Postup:
1. stáhněte si do telefonu nejnovější ATC [Firmware](https://github.com/atc1441/ATC_MiThermometer/releases)
2. otevřete v chrome na telefonu tento [Webflasher](https://atc1441.github.io/TelinkFlasher.html)
3. klikněte na "Connect"
4. vyberte soubor z telefonu pomocí tlačítka "procházet"
5. stiskněte "Do Activation"
6. dále "Start flashing"
7. jakmile uvidíte v logu "Update done" je hotovo
8. opět dejte "Connect" a v seznamu dostupných zařízeních se objeví ATC_XXXXXX
9. opiště si tento název (hodnoty pod XXXXXX je konec MAC adresy - budete potřebovat)
10. nyní můžete upravit nastavení firmwaru pomocí tlačítek na konci
11. můžete například vypnout smajlík a nastavit interval aktualizace senzoru 
12. nedávejte "Adversing Type: Mi Like" jinak nebude fungovat konfigurace v ESPHome
13. pro uložení nastavení stiskněte tlačítko "Save current settinfs in flash"
14. nakopírujte do ESPHome config níže a upravte MAC adresu
15. MAC adresa začíná vždy A4:C1:38 a druhou polovinu doplňíte z názvu ATC, který jste si poznamenali 
16. Příklad - ATC_93:25:D9 = A4:C1:38 + 93:2B:D9 = A4:C1:38:93:2B:D9

## ESPHome config:
```
bluetooth_proxy:
  active: true

sensor:
  - platform: atc_mithermometer
    mac_address: "A4:C1:38:93:2B:D9"
    temperature:
      name: "BLE teplota"
#      filters: # pouze pokud potrebujes
#        - calibrate_linear:
#            - 0.0 -> 0.2
#            - 10.0 -> 10.2
    humidity:
      name: "BLE vlhkost"
    battery_level:
      name: "BLE baterie"
    battery_voltage:
      name: "BLE baterie napětí"
    signal_strength:
      name: "BLE Signal"
```
Zdroje:
 [ESPHome](https://esphome.io/components/sensor/xiaomi_ble.html)
 [Firmware](https://github.com/atc1441/ATC_MiThermometer/releases)
 [ATC_MiThermometer](https://github.com/atc1441/ATC_MiThermometer)
