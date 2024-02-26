## Hi team! <br />
**From version 2.2 3X-UI make warning for me, that I am not use TLS when I communicate with WebPanel.**<br />
I know, that it is security issue for me, but I can't use LetsEncrypt certificate, cause I am not use domains. <br />
Solution - make self-signed certificate for my IP address.<br />
## Lets do this!<br />
Go to server bash.<br />
I create folder inside /usr/local/x-ui/ directory for cert's.<br />
`cd /usr/local/x-ui`<br />
`mkdir ssl-srt`<br />
`cd ssl-srt`<br />
Next step - create certificates. I make it for 10 years, if you need more or less - just change -days option.<br />
`openssl req -x509 -newkey rsa:4096 -keyout key.pem -out cert.pem -sha256 -days 3650`<br />
### **WARN**<br />
It will ask you for PEM pass phrase - remember it! It will bee needed for next step! Or use easy - I use "1234"<br />
![1](https://github.com/MHSanaei/3x-ui/assets/83214353/7dec3195-4eaa-4ea8-a486-e1b4ca2115a5)<br />
Next step - input data for certificate. I leave all blank. <br />
**!!!BUT!!!** <br />
You need to input **your real IP address into Common Name field**. <br />
![2](https://github.com/MHSanaei/3x-ui/assets/83214353/7dcb0a8c-6599-44a8-80d7-e6e35e4c827f)<br />
Certs made. But if you try to install it inside the panel, you will take an error, cause your private key locked by pass phrase.<br />
Let's unlock it! <br />
`openssl rsa -in key.pem -out key.un.pem -passin pass:YOUR PASS PHRASE` <br />
![image](https://github.com/MHSanaei/3x-ui/assets/83214353/bd8609f5-7ac1-4831-ac36-b32e46f8a282) <br />
## And the final round - install it into the WebPanel.<br />
![image](https://github.com/MHSanaei/3x-ui/assets/83214353/5268e88a-d7fa-4429-93ba-f07ea3958ca4)

## Final
After save and reboot Webpanel you will take an error about self-signed certificate, just ignore it. But, you will not see TLS error inside - your connection will be encrypted! <br /> 
All complete! <br />
Good Luck! <br />
