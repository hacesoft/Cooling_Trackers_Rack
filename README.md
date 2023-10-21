# Cooling_Trackers_Rack
Cooling of Trackers in the Rack



```
//*****************************************************************************
//
// File Name        : 'Chlazeni MPTT trakeru'
// Title            : Cooling of Trackers in the Rack
// Author           : http://www.prochazka.zde.cz -> hacesoft 2023
// Created          : 21-10-2023, 11:00
// Revised          : 21-10¨8-2023, 18:58
// Version          : 1.00
// Target Platfirm  : Node-RED
//
// This code is distributed under the GNU Public License
// Vsechny informace jsou zahrnuty pod GPL licenci, pokud není explicitne uveden jiný typ licence.
// Pouzivání techto stránek ke komercním �celum lze jen se souhlasem autora.
// Vsechna práva vyhrazena (c) 1997 - 2023 hacesoft.
//
//*****************************************************************************
const nRelayNumber = 0;
const sIpAddress = "192.168.1.5";
var sTurn = "off";  // Výchozí hodnota
var nLastChangeTime = null;  // Proměnná pro sledování času od poslední změny
var nHysteresisTime = 300;  // Hystereze v sekundách

var nOutput1 = msg.payload.JV; // Výkon z prvého trackeru
var nOutput2 = msg.payload.WEST; // Výkon z druhého trackeru
var nCelkem = nOutput1 + nOutput2;

// Zkontroluj, zda nCelkem je větší než 1000 a jestli uplynulo alespoň čas hystereze od poslední změny
if (nCelkem >= 1000) {
    if (nLastChangeTime === null) {
        nLastChangeTime = Date.now();
    } else {
        const nCurrentTime = Date.now();
        const nTimeSinceLastChange = nCurrentTime - nLastChangeTime;
        if (nTimeSinceLastChange >= nHysteresisTime * 1000) {  // Převeď čas hystereze na milisekundy
            sTurn = "on";
            nLastChangeTime = null;  // Resetuj čas od poslední změny
        }
    }
} else {
    sTurn = "off";
    nLastChangeTime = null;  // Resetuj čas od poslední změny
}
msg._url = `http://${sIpAddress}/relay/${nRelayNumber}?turn=${sTurn}`;
return msg;
```
![Colling_node_red.png](./) Colling_node_red.png
```
[{"id":"6cf0017697520c44","type":"tab","label":"chlazeni MPTT trakeru","disabled":false,"info":"\r\n","env":[]},{"id":"6d949c46f8659f66","type":"http request","z":"6cf0017697520c44","name":"","method":"POST","ret":"txt","paytoqs":"ignore","url":"{{{_url}}}","tls":"","persist":false,"proxy":"","insecureHTTPParser":false,"authType":"","senderr":false,"headers":[],"x":670,"y":340,"wires":[[]]},{"id":"d436d62da80b86e4","type":"victron-input-solarcharger","z":"6cf0017697520c44","service":"com.victronenergy.solarcharger/278","path":"/Yield/Power","serviceObj":{"service":"com.victronenergy.solarcharger/278","name":"MPPT WEST"},"pathObj":{"path":"/Yield/Power","type":"float","name":"PV Power (W)"},"name":"WEST","onlyChanges":true,"roundValues":"0","x":70,"y":280,"wires":[["b82677e9f74aa933"]]},{"id":"acc00f93399a6275","type":"victron-input-solarcharger","z":"6cf0017697520c44","service":"com.victronenergy.solarcharger/279","path":"/Yield/Power","serviceObj":{"service":"com.victronenergy.solarcharger/279","name":"MPPT VJ"},"pathObj":{"path":"/Yield/Power","type":"float","name":"PV Power (W)"},"name":"JV","onlyChanges":true,"roundValues":"0","x":70,"y":340,"wires":[["b82677e9f74aa933"]]},{"id":"963735aa2c2813dd","type":"function","z":"6cf0017697520c44","name":"SUB_TrackerS","func":"//*****************************************************************************\n//\n// File Name        : 'Chlazeni MPTT trakeru'\n// Title            : Cooling of Trackers in the Rack\n// Author           : http://www.prochazka.zde.cz -> hacesoft 2023\n// Created          : 21-10-2023, 11:00\n// Revised          : 21-10¨8-2023, 18:58\n// Version          : 1.00\n// Target Platfirm  : Node-RED\n//\n// This code is distributed under the GNU Public License\n// Vsechny informace jsou zahrnuty pod GPL licenci, pokud není explicitne uveden jiný typ licence.\n// Pouzivání techto stránek ke komercním �celum lze jen se souhlasem autora.\n// Vsechna práva vyhrazena (c) 1997 - 2023 hacesoft.\n//\n//*****************************************************************************\nconst nRelayNumber = 0;\nconst sIpAddress = \"192.168.1.5\";\nvar sTurn = \"off\";  // Výchozí hodnota\nvar nLastChangeTime = null;  // Proměnná pro sledování času od poslední změny\nvar nHysteresisTime = 300;  // Hystereze v sekundách\n\nvar nOutput1 = msg.payload.JV; // Výkon z prvého trackeru\nvar nOutput2 = msg.payload.WEST; // Výkon z druhého trackeru\nvar nCelkem = nOutput1 + nOutput2;\n\n// Zkontroluj, zda nCelkem je větší než 1000 a jestli uplynulo alespoň čas hystereze od poslední změny\nif (nCelkem >= 1000) {\n    if (nLastChangeTime === null) {\n        nLastChangeTime = Date.now();\n    } else {\n        const nCurrentTime = Date.now();\n        const nTimeSinceLastChange = nCurrentTime - nLastChangeTime;\n        if (nTimeSinceLastChange >= nHysteresisTime * 1000) {  // Převeď čas hystereze na milisekundy\n            sTurn = \"on\";\n            nLastChangeTime = null;  // Resetuj čas od poslední změny\n        }\n    }\n} else {\n    sTurn = \"off\";\n    nLastChangeTime = null;  // Resetuj čas od poslední změny\n}\nmsg._url = `http://${sIpAddress}/relay/${nRelayNumber}?turn=${sTurn}`;\nreturn msg;","outputs":1,"noerr":0,"initialize":"","finalize":"","libs":[],"x":460,"y":340,"wires":[["6d949c46f8659f66"]]},{"id":"b82677e9f74aa933","type":"join","z":"6cf0017697520c44","name":"","mode":"custom","build":"object","property":"payload","propertyType":"msg","key":"topic","joiner":"\\n","joinerType":"str","accumulate":true,"timeout":"","count":"2","reduceRight":false,"reduceExp":"","reduceInit":"","reduceInitType":"","reduceFixup":"","x":270,"y":340,"wires":[["963735aa2c2813dd"]]}]
```
