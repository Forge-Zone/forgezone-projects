let’s hook up the spotify API real quick so we can pull in all those juicy stats 😮‍💨

1. Go to Spotify API Dashboard [https://developer.spotify.com/dashboard](https://developer.spotify.com/dashboard), and login with your spotify account
2. Create a new app
3. Fill anything in app name and description
4. in redirect uri add this: **[http://localhost:3000/api/auth/callback**](http://localhost:3000/api/auth/callback**) we’ll explain why in later modules, after filling make sure to press add
5. check Web API

In the end your page should look something like this

![image.png](https://i.postimg.cc/CKKNkW6D/1.png)

1. press save

You will be redirected to something like

![image.png](https://i.postimg.cc/0N3pmrSR/2.png)

1. Go to the settings
2. here you can see **the client ID** copy it

![image.png](https://i.postimg.cc/cCGw9jTF/3.png)

1. go back to vscode and _rename .env.example to .env_ and add your spotify client id

![image.png](https://i.postimg.cc/WbPwXDHs/4.png)

1. Go back to spotify page and press client secret, a new code will pop up copy this and add this in your env file

![image.png](https://i.postimg.cc/xdqvcLc3/5.png)

1. in your .env add a new variable named NEXT_URL this will be our base url in the development build we’ll keep it [localhost](http://localhost) and we’ll deploy it we’ll change it

![image.png](https://i.postimg.cc/9QmtH62S/6.png)

In the end your env file should be something like this with different values

you can leave gemini key like this only we’ll add it in the future

**make sure to this all these setup nicely, if you missed something then you will be having errors in next modules, I recommend to recheck if you did every step correct**

## **Please do this or Shrit will be sad.**

Go ahead and send **“Spotify API Setup Done”** in `#progress` (in discord) to inspire others who are searching for cool use cases.