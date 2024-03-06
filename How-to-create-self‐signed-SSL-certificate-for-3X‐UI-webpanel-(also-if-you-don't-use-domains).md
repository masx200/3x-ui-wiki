## Hi team! <br />
**From version 2.2 3X-UI make warning for me, that I am not use TLS when I communicate with WebPanel.**<br />
I know, that it is security issue for me, but I can't use LetsEncrypt certificate, cause I am not use domains. <br />
Solution - make self-signed certificate for my IP address.<br />
## Lets do this!<br />
Go to server bash.<br />
I create folder inside /root directory for cert's.<br />
`cd /root`<br />
`mkdir ssl-srt`<br />
`cd ssl-srt`<br />
Next step - create certificates. I make it for 10 years, if you need more or less - just change -days option.<br />
`openssl req -x509 -newkey rsa:4096 -keyout key.pem -out cert.pem -sha256 -days 3650`<br />
### **WARN**<br />
It will ask you for PEM pass phrase - remember it! It will bee needed for next step! Or use easy - I use "1234"<br />
![image](https://github.com/MHSanaei/3x-ui/assets/83214353/cb469abd-36f8-4119-aee6-412ba725f4f8)<br />
Next step - input data for certificate. I leave all blank. <br />
**!!!BUT!!!** <br />
You need to input **your real IP address into Common Name field**. <br />
![image](https://github.com/MHSanaei/3x-ui/assets/83214353/fb00a85f-e979-41d4-93c2-5230dcdb8d7f)<br />
Certs made. But if you try to install it inside the panel, you will take an error, cause your private key locked by pass phrase.<br />
Let's unlock it! <br />
`openssl rsa -in key.pem -out key.un.pem -passin pass:YOUR PASS PHRASE` <br />
![image](https://github.com/MHSanaei/3x-ui/assets/83214353/bd83446d-3b80-46c3-a09c-b0eb70f0268f)<br />
## And the final round - install it into the WebPanel.<br />
![image](https://github.com/MHSanaei/3x-ui/assets/83214353/0430fd04-3722-4883-be35-b44cd5fdfc2b)

## Final
After save and reboot Webpanel you will take an error about self-signed certificate, just ignore it. But, you will not see TLS error inside - your connection will be encrypted! <br /> 
All complete! <br />
Good Luck! <br />
