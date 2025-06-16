alright we have done the setup now let’s move to the real deal 😈😈😈

in your `app/page.tsx` change the url to `/api/auth`

```tsx

const Home = () => {
  return (
    <div className="container">
      <div className="header">
        <div className="header-title">
          <h1>Rude Spotify Rewind</h1>
        </div>
        <div className="header-subtitle">
          <h2>Exposing your terrible music taste with zero mercy</h2>
        </div>

        <div className="button-container">
          {/* make this to /api/auth */}
          <Link href="/api/auth">
            <button className="button">Login with Spotify</button>
          </Link>
        </div>
      </div>
    </div>
  );
};
```

cool now let’s get started by writing api code

this is something which i really like about NEXT.JS it allows you to use api inside the same application no need differentiate between your backend and frontend (if you have used MERN you’ll probably know about it), this feature is known as serverless function BTW

## Starting with Backend

so let’s get started **in your `app` folder create a new folder as `api` inside create a new folder `auth` and then inside create a file known as `route.ts`**

make sure you do this right, we have just created a serverless function

just to verify you should be in `app/api/auth/route.ts`

```jsx
export async function GET() {
  // as soon as we reach /api/auth, this function will be called
}
```

if you have worked in express before we write GET and POST functions that’s what we’re doing if you don’t know then a simple google search will tell you about it

alright so **what’s our goal?**

we want as soon login is clicked the user is redirected to a spotify url where they can login after logging in we redirect them to a new page this page will be our main page and we’ll get all the spotify data in the new page cool….

but this was the overview let’s go little more deep

1. when the button is clicked we need to redirect user in the to spotify url for logging in
2. when the user in logged in remember in previous section we had redirect user as `/api/auth/callback` we’re gonna write a get function inside this
3. in this function we’re gonna get somthing known as token you can say it’s a little key for us to get user data
4. now we will need to store this token data somewhere inorder to be able to use it anywhere in our application we’re gonna store in localstorage using [cookies](https://youtube.com/shorts/xCcPIvdrnPE?si=2MPOlvi7eCppNI6B)

## Logging In With Spotify

now in you root directory you’ll find a folder with name as `lib` there will be some files already their, we’re gonna use these files in the future for now create a new file as `spotify.ts`

```tsx
// getting clientId and redirectUri from env
const CLIENT_ID = process.env.SPOTIFY_CLIENT_ID!;
const REDIRECT_URI = `${process.env.NEXT_URL}/api/auth/callback`;
export async function getSpotifyAuthURL() {
  // getting scoope to get permissions for our rewind
  const scope =
    "user-top-read user-read-email user-follow-read user-read-private user-read-recently-played";

  //giving some data to the spotify to help us get the auth code
  const params = new URLSearchParams({
    response_type: "code",
    client_id: CLIENT_ID,
    scope,
    redirect_uri: REDIRECT_URI,
  });

  // redirecting to the spotify auth page
  return `https://accounts.spotify.com/authorize?${params.toString()}`;
}
```

cool from here we have a function which we can use in our `api/auth/route.ts`

so in the file import spotify.ts and then we’re gonna send user to the url

```tsx
// getting imports
//redirect is a function provided by next, simpliy it's <a> for api's 
import { redirect } from "next/navigation"; 
import { getSpotifyAuthURL } from "@/lib/spotify";

export async function GET() {
  // getting the url
  const authUrl = await getSpotifyAuthURL();

  //   redirecting the url
  redirect(authUrl);
}
```

alright now when you click on the login button you’re gonna see the spotify auth and once you do it you’re gonna see something like this

![image.png](https://i.postimg.cc/LX7dPk9k/image.png)

this is because we have to create callback

**but if you see this LET’S GO WE’RE HALFWAY DONE WOHO 🎉🎉**

## Getting and Storing Token

in your `api/auth` folder create a new folder `callback` and create a new file as `route.ts`

```tsx
// imports
import { cookies } from "next/headers"; //storing cookies with next is more easy than doing it manually
import { redirect } from "next/navigation";

export async function GET(request: Request) {
  // whenever spotify redirects back to us, it will include a code in the URL
  const { searchParams } = new URL(request.url);
  const code = searchParams.get("code");

  if (!code) {
    redirect("/");
  }

  //   through the code we can get the access token and refresh token
  const response = await fetch("<https://accounts.spotify.com/api/token>", {
    method: "POST",
    headers: {
      "Content-Type": "application/x-www-form-urlencoded",
      Authorization: `Basic ${Buffer.from(
        `${process.env.SPOTIFY_CLIENT_ID}:${process.env.SPOTIFY_CLIENT_SECRET}`
      ).toString("base64")}`,
    },
    body: new URLSearchParams({
      grant_type: "authorization_code",
      code,
      redirect_uri: `${process.env.NEXT_URL}/api/auth/callback`,
    }),
  });

  const data = await response.json();

  if (data.error) {
    redirect("/");
  }

  //   store the access token and refresh token in cookies
  const cookieStore = await cookies();
  cookieStore.set("spotify_access_token", data.access_token, {
    maxAge: 3600,
    path: "/",
  });
  cookieStore.set("spotify_refresh_token", data.refresh_token, {
    maxAge: 3600 * 24 * 30,
    path: "/",
  });

  redirect("/rewind"); //when we're done going to /rewind page
}
```

when you look code first time it might kill you (from panic attack) but it’s not hard, just read my comments you’re gonna get we are just doing what we wrote in bullet points above

now if you try it again, if everything works fine you’re gonna redirect to this page

![image.png](https://i.postimg.cc/fRZbksgz/image2.png)

if you’re not seeing this and having some errors, do these steps in order

1. if you can see the error msg just copy and paste it on google, 80% of the issues get resolved
2. go to discord and in the `#core-1` section and and ask about your issue, if noone is live
3. just ask AI, keep this as last options because they’re not really that helpful in debugging imo, LLm’s are pretty random sometime it works sometime it doesn’t

and if you’re seeing this page and saying where did this page even come from then if you look closely then we have a rewind folder inside `app/rewind/page.tsx` you can have your name here

```jsx
export default function RewindPage() {
  return (
    <div className="container max-w-7xl mx-auto px-4">
      <div className="header">
        <div className="header-title mb-6">
          {/* you name here */}
          <h1 className="text-4xl font-bold">Hello Shrit</h1>
        </div>
      </div>
    </div>
  );
}
```

![image.png](https://i.postimg.cc/3rsN5mXD/image3.png)

## One More Thing

now I want that we can easily get the token by one line of code because we’re gonna use it a lot so in your `lib/spotify.ts`

```jsx
// importing cookies
import { cookies } from "next/headers";

// previous code....

// code to get token in one line
export async function getAccessToken() {
  // Client-side token access
  if (typeof window !== "undefined") {
    // Running in browser
    const cookies = document.cookie.split(";");
    const tokenCookie = cookies.find((cookie) =>
      cookie.trim().startsWith("spotify_access_token=")
    );
    if (tokenCookie) {
      return tokenCookie.split("=")[1];
    }
    return null;
  } else {
    // Server-side token access
    const cookieStore = cookies();
    const token = (await cookieStore).get("spotify_access_token")?.value;
    return token || null;
  }
}

```

perfect now we can just get token in one line in something like

```jsx
const token = await getAccessToken();
```

cool we’re done for this section in next section we are actually gonna get some stats

## **Please do this or Shrit will be sad.**

Go ahead and take a screenshot of your rewind page. Share it in #progress to inspire others who are searching for cool use cases.