alright in some way we have been just setting up our project until now let’s write some real code now 😈😈

## The Game Plan

1. alright we have token which we can access
2. we’re gonna blindly follow [spotify documentation](https://developer.spotify.com/documentation/web-api)
3. we’re gonna write multiple small functions which retrieve small functions and in the end we’ll club it

this is gonna very long section hope you have you coffee ready 👹

## Before we begin

in `lib` create a new file as `actions.ts` this is where we’ll write our spotify functions so I want this run this before our page so add, the following line

```jsx
'use server'
```

and we’re gonna get all the functions in our `app/rewind/page.tsx` so on the top add

![image.png](https://i.postimg.cc/8cdzbkyz/1.png)

this is a NEXT.JS thingy a google search will tell enough

## Getting User Data

alright in our `actions.ts`

```jsx
"use server";
// importing packages
import { getAccessToken } from "@/lib/spotify";

export async function getUserProfile() {
  const token = await getAccessToken();

  if (!token) return null;

  //   a simple fetch request to the Spotify API to get the user profile
  const response = await fetch("<https://api.spotify.com/v1/me>", {
    headers: {
      Authorization: `Bearer ${token}`,
    },
  });

  if (!response.ok) return null;
  return response.json();
}
```

this function will give us our user data, [you can read the docs](https://developer.spotify.com/documentation/web-api/reference/get-current-users-profile) to know more

awesome now in our `rewind/page.tsx`

```jsx
"use client";
import { getUserProfile } from "@/lib/actions";
import React, { useState, useEffect } from "react";

export default function RewindPage() {
  // a user profile state to store the user profile data
  // useerProfile is an object you can have more in lib/types.ts
  const [userProfile, setUserProfile] = useState<UserProfile | null>(null);

  // as soon as page loads, we will fetch the user profile data
  useEffect(() => {
    // we have to create a function to fetch the data because we can't use async/await directly in useEffect
    const fetchData = async () => {
      // getting the user profile data from the actions.ts file
      const profile = await getUserProfile();

      //  we're setting the user profile data to the state
      setUserProfile(profile);
    };

    fetchData();
  }, []);

  console.log("userProfile", userProfile);

  return (
    <div className="container max-w-7xl mx-auto px-4">
      <div className="header">
        <div className="header-title mb-6">
          <h1 className="text-4xl font-bold">
            {/* as soon as pages loaded it will display your name */}
            Hello {userProfile?.display_name}
          </h1>
        </div>
      </div>
    </div>
  );
}
```

a lot is going on in this code make sure to read comments if something feels little weird to you, make sure you google it, imo best way to learn

if still you don’t get it you know where to reach us

ALRIGHT now if you make do this and now make sure to start from home page and then login again because after a certain time tokens expire and we need to login again

if you login in again and open your console you can see something like this

![image.png](https://i.postimg.cc/26hkz8N7/2.png)

awesome we’re on the right track

also make sure to read `lib/types.ts` it’s a typescript thing I already made you guys a interface object of everything we’re gonna use this will make things very easy for us

## Top Tracks

honestly it’s the same step

in our actions.ts

```tsx
export async function getTopTracks() {
  const token = await getAccessToken();

  if (!token) {
    console.error("getTopTracks: No access token available");
    return null;
  }

  try {
    const response = await fetch(
      `https://api.spotify.com/v1/me/top/tracks?limit=20&time_range=long_term`,
      {
        headers: {
          Authorization: `Bearer ${token}`,
        },
      }
    );

    if (!response.ok) {
      console.error(
        `getTopTracks: API error ${response.status} for time range long_term`
      );
      return null;
    }

    return response.json();
  } catch (error) {
    console.error(`Error in getTopTracks for time range long_term`, error);
    return null;
  }
}
```

refer to spotify docs I am using everything from their also we have a limit of 50 songs, but we’re gonna top 20 songs to give it to our AI this way we’ll have more data,

more data = more crazy results

yep though we’re gonna show only top 3 songs having 20 is cool

now in your `rewind/page.tsx`

```jsx
"use client";
// update this
import { getTopTracks, getUserProfile } from "@/lib/actions";
import React, { useState, useEffect } from "react";

export default function RewindPage() {
  const [userProfile, setUserProfile] = useState<UserProfile | null>(null);
  // add this
  const [topTracks, setTopTracks] = useState<TopTracksResponse | null>(null);

  useEffect(() => {
    const fetchData = async () => {
      // update this
      const profile = await getUserProfile();
      const topTracks = await getTopTracks();

      setUserProfile(profile);
      setTopTracks(topTracks);
    };

    fetchData();
  }, []);

  // log it
  console.log("Top Tracks:", topTracks);
  
  //.... rest of the code  
```

awesome now if you log it you’re gonna have a list of top 20 tracks awesomeeeeee

## Loading State

now if you see it’s annoying that it just shows Hello till it’s loading rightt let’s make a loading state, so that till the time data is loading we know it’s loading and nothing is breaking

```tsx
export default function RewindPage() {
  const [userProfile, setUserProfile] = useState<UserProfile | null>(null);
  const [topTracks, setTopTracks] = useState<TopTracksResponse | null>(null);
  // have a loading state
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    // turn the loading state on and off
    const fetchData = async () => {
      setLoading(true);
      const profile = await getUserProfile();
      const topTracks = await getTopTracks();

      setUserProfile(profile);
      setTopTracks(topTracks);
      setLoading(false);
    };

    fetchData();
  }, []);

  return (
    <>
      {/* if the loading is true then load the loader else load the page */}
      {loading ? (
        <div className="flex flex-col items-center justify-center min-h-screen py-12">
          <div className="loader" />
          {/* loader is a class which we have written in globa.css */}
        </div>
      ) : (
        <div className="container max-w-7xl mx-auto px-4">
           {/* rest of the code.......*/}
        </div>
      )}
    </>
  );
}
```

now you’ll have a pretty cool loading state

now I am not gonna show you how to write everything one by one cause it’s gonna take ages

[**we’re gonna repeat these tasks again and again you can copy the rest of the code here**](https://gist.github.com/Shrit1401/b80ab92111cc86aa80207d991929a60a)

after downloading make sure to open your log if you can se 5 data types you’re good to go

![image.png](https://i.postimg.cc/kXnJmm1W/3.png)

alright I wanna show you to create something like this a popular music genre now if you see spotify docs we don’t really have a way to get this so how are we going to do it?

![image.png](https://i.postimg.cc/cJK1bBDj/4.png)

alright if you closely see your data in the log you’ll see, that we have a array of genres so if we take all these genres we’ll have an idea of what a person listens

![image.png](https://i.postimg.cc/902Wz5W5/5.png)

cool now let’s code something like this

in your `actions.ts`

```jsx
// previous code ......

export const getPopularGenres = async () => {
  const token = await getAccessToken();

  if (!token) return null;

  // get all top artists
  const response = await fetch(
    `https://api.spotify.com/v1/me/top/artists?limit=50&time_range=long_term`,
    {
      headers: {
        Authorization: `Bearer ${token}`,
      },
    }
  );

  if (!response.ok) return null;

  const data = await response.json();

  // filter out the genres
  const genres = data.items.flatMap((artist: any) => artist.genres);

  // counting the genres
  const genreCount = genres.reduce((acc: any, genre: string) => {
    acc[genre] = (acc[genre] || 0) + 1;
    return acc;
  }, {});

  // sorting the genres and returning the top 5
  return Object.entries(genreCount)
    .sort((a: any, b: any) => b[1] - a[1])
    .slice(0, 5);
};
```

and now you’re `rewind/page.tsx`

```jsx
"use client";
//import
import {
  getRecentlyPlayed,
  getNewDiscovery,
  getTopArtists,
  getTopTracks,
  getUserProfile,
  getPopularGenres,
} from "@/lib/actions";
import React, { useState, useEffect } from "react";

export default function RewindPage() {
  const [userProfile, setUserProfile] = useState<UserProfile | null>(null);
  const [topTracks, setTopTracks] = useState<TopTracksResponse | null>(null);
  const [topArtists, setTopArtists] = useState<TopArtistsResponse | null>(null);
  const [newDiscovery, setNewDiscovery] = useState<NewDiscover | null>(null);
  // the genres type is little bit tricky we are using the unknown type to
  // avoid type errors
  const [popularGenres, setPopularGenres] = useState<
    [string, unknown][] | null
  >(null);
  const [recentlyPlayed, setRecentlyPlayed] =
    useState<RecentlyPlayedResponse | null>(null);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    const fetchData = async () => {
      setLoading(true);
      try {
        const profile = await getUserProfile();
        const topTracks = await getTopTracks();
        const topArtists = await getTopArtists();
        const recentlyPlayed = await getRecentlyPlayed();
        const topAlbums = await getNewDiscovery();
        const popularGenres = await getPopularGenres();

        setTopTracks(topTracks);
        setUserProfile(profile);
        setTopArtists(topArtists);
        setRecentlyPlayed(recentlyPlayed);
        setNewDiscovery(topAlbums);
        setPopularGenres(popularGenres);
      } catch (error) {
        console.error("Error fetching user profile:", error);
        setUserProfile(null);
      } finally {
        setLoading(false);
      }
    };

    fetchData();
  }, []);

  // removing other logs and only logging the popular genres
  console.log(popularGenres);
  
  // code .......
```

![image.png](https://i.postimg.cc/Vs9fp6RY/7.png)

something like this will be shown in the log

and that will be it we have successfully taken all the data we’ll need from spotify now we’ll add AI to make the facts fascinating, cool I am getting builder’s vibe let’s gooo

## **Please do this or Shrit will be sad.**

Go ahead and take a screenshot of your rewind page with your display name. Share it in #progress to inspire others who are searching for cool use cases.