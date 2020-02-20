# GSM-Modem
A Python GSM modem implementation based on the SIM868 specification

## API Summary
| Method | Description&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; | Arguments&nbsp;&nbsp;&nbsp;&nbsp; | Hayes Commands |
|--------|-------------|-----------|----------------|
| write  | Send a Hayes command to the modem | `command, timeout = 5` | |
| read   | Reads a response from the modem   | | |
| getEcho | Get an echo response from the modem | | `AT` |
| getCSQ | Get the call signal quality | | `AT+CSQ` |
| getText | Get an SMS text message from the modem | | `AT+CMGR` |
| sendText | Send an SMS text message | `number, message` | `AT+CMGF=1` `AT+CSCS="GSM"` `AT+CMGS` |
| setGPSOn | Turn the GNSS/GPS on | | `AT+CGNSPWR=1` |
| getGPSData | Get the GNSS/GPS data from the modem | | `AT+CGNSINF` |
| httpPost | HTTP post to a url | `url` | `AT+HTTPINIT` `AT+HTTPPARA` `AT+HTTPACTION=0` `AT+HTTPTERM` |
| httpInit | Initialize the HTTP client | | `AT+HTTPPARA="CID",1` `AT+SAPBR=3,1,"CONTYPE","GPRS"` `AT+SAPBR=3,1,"APN","pp.vodafone.co.uk"` `AT+SAPBR=3,1,"USER","wap"` `AT+SAPBR=3,1,"PWD","wap"` `AT+SAPBR=2,1` `AT+SAPBR=1,1` |

### getCSQ
| Value | Definition |
|-------|------------|
| 0 | -115 dBm or less |
| 1 | -111 dBm |
| 2...30 | -110... -54 dBm |
| 31 | -52 dBm or greater |
| 99 | not known or not detectable |

### sendText
The `number` argument takes phone numbers in the E.164 format, for example +447797123456

### setGPSOn
The method only needs to be called once to turn the GNSS/GPS functionality on

### getGPSData
The method returns a string-indexed array. The keys are `datetime`, `latitude`, `longitude` and `altitude`

### httpInit
The method only needs to be called once to initialize parameters for future HTTP interactions. The modem is configured to use Vodaphone UK's configuration; this can be easily changed via the modem code

## Examples
The below example opens a serial connection to the modem via `COM3`, prints an echo response from the modem, prints the current call signal quality, turns the GNSS/GPS on and then prints the GNSS/GPS data to the console every 60 seconds

```
def main():
	modem = GSMModem('COM3')

	print("Echo = " + modem.getEcho())
	print("CSQ = " + modem.getCSQ())

	time.sleep(1)

	modem.setGPSOn()

	while True:
		gps = modem.getGPSData()

		print("GPS Data = " + str(gps))

		time.sleep(60)


if __name__ == '__main__':
	main()
```
