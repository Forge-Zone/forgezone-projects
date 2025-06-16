## Setup
okay to setup our API we’re gonna first get the API Key
1. go to [https://aistudio.google.com/](https://aistudio.google.com/)
2. go to the dashboard section
![image ](https://i.postimg.cc/qMxyQ4YY/image.png)

1. Click on create API Key Button
2. create a API key now you’ll need a google cloud projects it isn’t hard create one
![image.png](https://i.postimg.cc/4ytbXcWN/2.png)

tldr: figure out and get the API Key

in your .env paste the API key

your .env should be looking something like this
![image.png](https://i.postimg.cc/CxJCMSsD/3.png)


make sure you type the NEXT_PUBLIC_GOOGLE_GEMINI_API_KEY correct we’re gonna reveal this to our client side

let’s get google genai package, in the terminal type

```jsx
npm i @google/genai
```

![image.png](https://i.postimg.cc/rpm1K3fH/4.png)