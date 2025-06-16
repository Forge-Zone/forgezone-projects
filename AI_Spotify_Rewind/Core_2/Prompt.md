let’s get to the real shit, prompting

## Card

before we start with AI let’s make a card for our spotify in our `rewind page`

```jsx
...
<div className="container max-w-7xl mx-auto px-4">
  <div className="header">
    <div className="header-title mb-6">
      <h1 className="text-4xl font-bold">Hello {userProfile?.display_name}</h1>
    </div>

    <div className="bg-gradient-to-r bg-[#111] border-white/50 border-2 border-dashed rounded-xl shadow-lg p-8 mb-10">
      <div className="flex items-center space-x-6">
        <img
          src={
            userProfile.images &&
            userProfile.images[0] &&
            userProfile.images[0].url
          }
          alt="Profile picture"
          className="rounded-full w-24 h-24 border-2"
          width={96}
          height={96}
        />
        <div className="text-white">
          <h2 className="text-2xl font-bold">{userProfile.display_name}</h2>
          <p className="text-indigo-100">{userProfile.email}</p>
        </div>
      </div>

      <div className="mt-6 grid grid-cols-3 gap-6">
        <div className="p-3 bg-black/30 rounded-lg">
          <p className="text-gray-400">Tracks</p>
          <p className="text-2xl font-bold">{topTracks.limit}</p>
        </div>
        <div className="p-3 bg-black/30 rounded-lg">
          <p className="text-gray-400">Artists</p>
          <p className="text-2xl font-bold">{topArtists.limit}</p>
        </div>
        <div className="p-3 bg-black/30 rounded-lg">
          <p className="text-gray-400">Genres</p>
          <p className="text-2xl font-bold">
            {popularGenres ? Object.keys(popularGenres).length : 0}
          </p>
        </div>
      </div>
    </div>
  </div>
</div>
....
```

if you’re getting many undefined errors then you can just add false bools

![Screenshot 2025-06-10 at 5.07.30 PM.png](https://i.postimg.cc/HW5Djsvf/Screenshot-2025-06-10-at-5-07-30-PM.png)

cool now you see the page it’s so cool

![Screenshot 2025-06-10 at 5.09.16 PM.png](https://i.postimg.cc/s2NdGBrR/2.png)

## How to Prompt

1. we’re gonna create a function which generates text with gemini
2. write a prompt where we mention
    1. detailed idea of what we want
    2. data which we have gathered
    3. nice idea of how we’re gonna get our data
3. get data from our prompt

let’s start

## 1

let’s create a function inside `gemini.ts` which generates text, btw i am using `gemini-1.5-flash` model

```jsx
export const geminiPrompt = async (prompt: string) => {
  if (!apiKey) {
    throw new Error("Google Gemini API key is not configured");
  }

  const response = await ai.models.generateContent({
    model: ai_model,
    contents: prompt,
  });
  return response.text;
};
```

cool

## 2

Alright, let's create a function which will generate our video facts. So, in our `gemini.ts` file, we are going to create a function for the same, we are also going to take some data and if you want to know about the type of data that we are taking, then you can open `types.ts`. This file contains all the types which we would be using. So using this, we would be creating an input in TypeScript, something like this.

```
export const generateVideoFacts = async (
  userProfile: UserProfile,
  topTracks: TopTracksResponse,
  topArtists: TopArtistsResponse,
  genres: [string, unknown][],
  recent: RecentlyPlayedResponse,
  newDiscovery: NewDiscover
) => {};
```

## 2a

now let’s write a prompt

remember y’all picked your ideas in core 1, we’re gonna use that this prompt will be diffrent for everybody, prompt should look something like

```
    [tell about your unique idea]
    Write short, punchy captions that will appear on a "Your Music Roast" style video.

    Don't use any year or date references.
    No mentions of Spotify or platforms.
    Use their tracks, artists, genres, and plays to create a savage narrative.
    
    Each line must:
    - Be between 3-5 words
    - No emojis, no markdown
    - Be humorously judgmental without crossing the line
    - Use their specific music choices to deliver stinging but funny observations
  
    Format strictly like:
    [some examples of the format]
```

for example the prompt for my rude rewind was

```
    As a brutally honest music critic with zero patience, create harshly accurate roasts for this Spotify user.
    Don't sugarcoat their questionable taste. Be sarcastic, witty, and slightly offensive - but keep it funny.
    Write short, punchy captions that will appear on a "Your Music Roast" style video.

    Don't use any year or date references, just focus on mocking their terrible music choices.
    No mentions of Spotify or platforms - focus on destroying their musical identity.
    Use their tracks, artists, genres, and plays to create a savage narrative.
    
    Each line must:
    - Be between 3-5 words
    - No emojis, no markdown
    - Be humorously judgmental without crossing the line
    - Use their specific music choices to deliver stinging but funny observations
  
    Format strictly like:
    Basic taste detected
    Really? This garbage?
    Taylor? How original
```

cool make sure to do this step in your own way, with going bullish on your idea, that way it’ll be fun when it generates something cool

in your `gemini.ts` create a const as prompt and add your prompt, inside the `generateFacts()` function

```tsx
const prompt = ``;
```

instead of normal `""` use backticks ```` this way we'll able to use string interpolation

Then add the prompt which we wrote in our 2A, which is basically the introduction prompt

```tsx
const prompt = `
    As a brutally honest music critic with zero patience, create harshly accurate roasts for this Spotify user.
    Don't sugarcoat their questionable taste. Be sarcastic, witty, and slightly offensive - but keep it funny.
    Write short, punchy captions that will appear on a "Your Music Roast" style video.

    Don't use any year or date references, just focus on mocking their terrible music choices.
    No mentions of Spotify or platforms - focus on destroying their musical identity.
    Use their tracks, artists, genres, and plays to create a savage narrative.
    
    Each roast must:
    - Be between 3-5 words
    - No emojis, no markdown
    - Be humorously judgmental without crossing the line
    - Use their specific music choices to deliver stinging but funny observations
  
    Format strictly like:
    Basic taste detected
    Really? This garbage?
    Taylor? How original
    `;
```

## 2b

Now let's give some data to our AI.

Now, before we start with the prompt, we have to define a few variables, which would be giving an empty array or an empty list if there is no data available in the Spotify. This is important because of two reasons.

1. if the user is new and he doesn't have heard anything, then these things might give an error,
2. if Spotify data API is down or something, then also we want that we can just return empty string and our app works just fine.

so do so in our function you can write something like

```tsx
	const safeNewDiscovery = newDiscovery?.items ? newDiscovery : { items: [] };
  const safeTopTracks = topTracks?.items ? topTracks : { items: [], total: 0 };
  const safeTopArtists = topArtists?.items
    ? topArtists
    : { items: [], total: 0 };
  const safeRecent = recent?.items ? recent : { items: [] };
  const safeGenres = genres || [];
```

awesome, So now you can very freely copy the prompt which is below. This will be the same for everyone and you can read through it. It isn't really that much complicated.

```jsx
		... the intro prompt before it
		`USER PROFILE:
	    Name: ${userProfile?.display_name || "Music Fan"}
	    New Discoveries: ${safeNewDiscovery.items
	      .slice(0, 20)
	      .map((item: any) => item?.name || "")
	      .filter(Boolean)
	      .join(", ")}
	    Tracks played: ${safeTopTracks.total}
	    Artists explored: ${safeTopArtists.total}
  
    TOP ARTISTS: ${safeTopArtists.items
      .slice(0, 20)
      .map((a: any) => a?.name || "")
      .filter(Boolean)
      .join(", ")}
  
    TOP TRACKS: ${safeTopTracks.items
      .slice(0, 20)
      .map((t: any) =>
        t?.name && t?.artists?.[0]?.name
          ? `${t.name} by ${t.artists[0].name}`
          : ""
      )
      .filter(Boolean)
      .join(", ")}
  
    FAVORITE GENRES: ${
      safeGenres.length > 0
        ? safeGenres
            .slice(0, 20)
            .map(([genre]: [string, unknown]) => genre || "")
            .filter(Boolean)
            .join(", ")
        : ""
    }
  
    RECENT DISCOVERIES: ${safeNewDiscovery.items
      .slice(0, 20)
      .map((t: any) =>
        t?.name && t?.artists?.[0]?.name
          ? `${t.name} by ${t.artists[0].name}`
          : ""
      )
      .filter(Boolean)
      .join(", ")}
  
    MOST RECENT PLAYS: ${safeRecent.items
      .slice(0, 20)
      .map((item: any) =>
        item?.track?.name && item?.track?.artists?.[0]?.name
          ? `${item.track.name} by ${item.track.artists[0].name}`
          : ""
      )
      .filter(Boolean)
      .join(", ")}`
```

## 2c

Now to generate prompt, we'll say AI to generate prompt in each line. And we can cut the line, and we will get different prompts for different things. So you can just copy the prompt in your own way, and paste it

```
Generate 6 short, punchy captions:
    1. [greeting]
    2. [Top tracks]
    3. [Top artists]
    4. [New discoveries]
    5. [Genre taste]
    6. [Recent plays]
```

You can also see my example

```
		Generate 6 short, roast captions:
    1. A sarcastic greeting
    2. Top tracks mockery
    3. Artist choice ridicule
    4. New discoveries critique
    5. Genre taste judgment
    6. Recent plays embarrassment
```

write something like this with accordance to your idea and put it inside your prompt

## 3

Now let's generate our facts with a Gemini prompt. So, you can just copy the code below. I have written comments which will be easy for you to read and understand the code. And in any way, you feel like you don't understand the code or you're lost, a Google search might be a good option. And if you see, still feel like that the issue is not resolved, hop onto Discord with us. We are always there for you.

```tsx
try {
    // generate captions from gemini
    const response = await geminiPrompt(prompt);
    if (!response) throw new Error("Failed to generate AI response");

    const extractInsight = (text: string): string => {
      // Remove leading emoji, dots or any punctuation at the start, then trim
      return text.replace(/^[^\\w\\s]*\\s*/u, "").trim();
    };

    // extract captions from gemini response
    const sections = response
      .split(/\\n/)
      .map((line) => line.trim())
      .filter((line) => line && line.length <= 60);

    // initialize video insights
    let videoInsights: VideoInsights = {
      hello: "[Analyzing your music identity...]",
      topTracks: "[Top tracks being processed...]",
      topArtists: "[Top artists insight pending...]",
      newdiscov: "[Discoveries are loading...]",
      genres: "[Genre reflection in progress...]",
      recentTracks: "[Your mood is processing...]",
    };

    // extract captions from gemini response
    if (sections.length >= 6) {
      videoInsights = {
        hello: extractInsight(sections[0]),
        topTracks: extractInsight(sections[1]),
        topArtists: extractInsight(sections[2]),
        newdiscov: extractInsight(sections[3]),
        genres: extractInsight(sections[4]),
        recentTracks: extractInsight(sections[5]),
      };
    }

    return videoInsights;
  } catch (error) {
    console.error("Error generating video insights:", error);
    return {
      hello: "[Analyzing your music identity...]",
      topTracks: "[Top tracks being processed...]",
      topArtists: "[Top artists insight pending...]",
      newdiscov: "[Discoveries are loading...]",
      genres: "[Genre reflection in progress...]",
      recentTracks: "[Your mood is processing...]",
    };
  }
```

Awesome! Now we are done with the Gemini, now it will give us a very detailed prompt. Feel free to change anything which you like.

I know today's code was a bit lengthy and complicated. That's why I'm providing the whole `gemini.ts` file in this, [click here for that](https://gist.github.com/Shrit1401/56d598eadd66f02c6881cb0688b2d722)

## **Please do this or Shrit will be sad.**

Go ahead and take a screenshot of prompt. Share it in `#progress` to inspire others who are searching for cool use cases.