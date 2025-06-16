Alright, first things first — if you’re reading this, that means you’ve actually made it to the final section of this entire dev phase, which is _honestly crazy_

So yeah, huge shoutout to you legends who’ve made it this far. Really, hats off. You didn’t just scroll — you _stuck around_. That’s some real builder energy. Mad respect. 💥

Alright, let’s get into it. But before we start, just a quick heads-up — I’ll only walk you through one or two sections of the code in detail. After that, I’ll drop the full code for you to copy-paste. Why? Because the logic is basically the same across the app — no point making you rewrite the same thing over and over again. Ain’t nobody got time for that.

Don’t worry though, the entire code will be loaded with clear comments, so it’ll be super easy to follow along.

## Clean Code

I have cleaned the code a bit from the previous section and you can have a starting point from here.

```typescript
import { AbsoluteFill, Sequence, spring, useCurrentFrame, useVideoConfig} from "remotion";
import React from "react";
import styled from "@emotion/styled";

import { AuroraBackground } from "./AuroraBackground";

const Logo = styled.img`
  position: absolute;
  bottom: 40px;
  left: 40px;
  width: 180px;
  height: auto;
  opacity: 0.9;
  z-index: 10;
`;

interface SpotifyRewindVideoProps {
  userProfile: UserProfile;
  topTracks: TopTracksResponse;
  topArtists: TopArtistsResponse;
  recentlyPlayed: RecentlyPlayedResponse;
  newDiscovery: any;
  popularGenres: [string, unknown][];
  videoInsights: VideoInsights;
}

const SpotifyRewindVideo = ({
  userProfile,
  topTracks,
  topArtists,
  recentlyPlayed,
  newDiscovery,
  popularGenres,
  videoInsights,
}: SpotifyRewindVideoProps) => {
	// we'll use this to animate the sections
  const frame: any = useCurrentFrame();
  // we'll use this to get the duration of the video
  const { durationInFrames } = useVideoConfig();

  // we'll use this to animate the sections
  const totalSections = 13;
  const sectionDuration = Math.floor(durationInFrames / totalSections);
  return (
    <AbsoluteFill style={{ background: "#1DB954" }}>
      <AuroraBackground
        style={{ position: "absolute", width: "100%", height: "100%" }}
      >
        <div style={{ width: "100%", height: "100%" }}></div>
      </AuroraBackground>
    </AbsoluteFill>
  );
};

export default SpotifyRewindVideo;
```

I have also added an interface prompt. This is just like what type is in TypeScript. And if you're wondering how we are entering these, then in our player function, we also had input props where we have already passed these things.

![image.png](https://i.postimg.cc/3wj6g7cn/1.png)

## Welcome Section

Now, let’s kick things off with our **first section — the Welcome Section**. This one’s a fun little start — it’ll show _your_ image, _your_ name, and a friendly little “hello & welcome” vibe.

![Screenshot 2025-06-13 at 5.20.59 PM.png](https://i.postimg.cc/wjLbpggg/Screenshot-20251-06-13-at-5-20-59-PM.png)

This image pretty much sums it up — every section will be its own Sequence, always with a title, sometimes with images, sometimes with extras, sometimes with neither. Depends on the section type, but yeah, that’s the base structure. so let’s have styling for these

```tsx
const Section = styled.div`
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: 100%;
  text-align: center;
  padding: 2rem;
  position: relative;
`;

const SectionTitle = styled.h1`
  font-size: 6rem;
  font-weight: 800;
  margin-bottom: 1.5rem;
`;

const Image = styled.img`
  max-width: 400px;
  max-height: 400px;
  width: auto;
  height: auto;
  border-radius: 50%;
`;
```

cool now for our first page we’ll have a section and inside we’ll have these things along with the styling

```tsx
const UserName = styled.p`
  font-size: 5rem;
  font-weight: 800;
  margin-top: 5rem;
`;
...
<Sequence from={0} durationInFrames={sectionDuration}>
  <Section>
    <div>
      <SectionTitle style={{ color: "#191414" }}>
        Welcome to Your Spotify Rewind
      </SectionTitle>
    </div>

    <div>
      <Image
        src={userProfile?.images?.[0]?.url}
        alt={userProfile?.display_name}
      />
    </div>

    <div>
      <UserName style={{ color: "#191414" }}>
        {userProfile?.display_name}
      </UserName>
    </div>

    <div>
      <Logo src="<https://i.imgur.com/65lwHJi.png>" alt="Forge Zone Logo" />
    </div>
  </Section>
</Sequence>;
```

![Screenshot 2025-06-13 at 5.24.09 PM.png](https://i.postimg.cc/zfkty7yQ/3.png)

when you add everything this will be visible awesome, animations will be in the code below as it’s really untidy

## Hello Msg

now let’s make a page which actually uses or AI generated text and shows it

```tsx
<Sequence from={sectionDuration} durationInFrames={sectionDuration}>
  <Section>
    <div>
      <SectionTitle style={{ color: "#191414" }}>
        {videoInsights.hello
          ? // we're removing the first 3 characters because they're numbers
            videoInsights.hello.slice(3)
          : "Hello, Spotify User!"}
      </SectionTitle>
    </div>
    <Logo src="<https://i.imgur.com/65lwHJi.png>" alt="Forge Zone Logo" />
  </Section>
</Sequence>;
```

awesome simple code but looks sick

## Top Tracks Msg

Nothing major in here. It would be similar to the last message. You can actually code it yourself, but I will give you a hint.

```tsx
<Sequence from={sectionDuration * 2} durationInFrames={sectionDuration}>
  <Section>
    <div>
      <SectionTitle style={{ color: "#191414" }}>
        {videoInsights.topTracks
          ? videoInsights.topTracks.slice(3)
          : "Your Top Tracks"}
      </SectionTitle>
    </div>

    <Logo src="<https://i.imgur.com/65lwHJi.png>" alt="Forge Zone Logo" />
  </Section>
</Sequence>;
```

If you’re wondering why we did sectionDuration * 2 — it’s simple. The from prop is like a delay. Since this is the third section, we need to wait for the first two to finish. Each one takes sectionDuration, so we just multiply it.

## Top Tracks

Alright, so for top tracks, we’re making a few changes. First, we’re adding new list classes — simple layout: image on the left, text on the right. Clean and clear. Also, every sections like popular tracks and artists should feel a bit different, so I’ve added some custom styles just for tracks.

```tsx
const List = styled.ul`
  display: flex;
  flex-direction: column;
  gap: 1.5rem;
  padding: 0;
  list-style: none;
  align-items: center;
`;

const ListItemImage = styled.img`
  max-width: 400px;
  max-height: 400px;
  width: auto;
  height: auto;
  border-radius: 10px;
  border: 4px solid rgba(255, 255, 255, 0.3);

  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
`;

const TrackName = styled.p`
  font-size: 2rem;
  font-weight: 800;
  color: #191414;
`;

const TrackArtist = styled.p`
  font-size: 1.5rem;
  font-weight: 800;
  opacity: 0.5;
  color: #191414;
`;
...
<Sequence from={sectionDuration * 3} durationInFrames={sectionDuration}>
  <Section>
    <SectionTitle>Top Tracks</SectionTitle>

    <List>
      {topTracks?.items?.slice(0, 3).map((track: any, index: number) => {
        return (
          <li
            key={track.id}
            className="flex flex-col items-center gap-2 space-x-6"
          >
            <ListItemImage
              src={track?.album?.images?.[0]?.url}
              alt={track?.name}
            />
            <div>
              <TrackName style={{ color: "#191414" }}>{track?.name}</TrackName>
              <TrackArtist>{track?.artists?.[0]?.name}</TrackArtist>
            </div>
          </li>
        );
      })}
    </List>

    <Logo
      src="<https://i.imgur.com/65lwHJi.png>"
      alt="Forge Zone Logo"
      style={{
        transform: `translateY(${
          Math.sin((frame - sectionDuration * 3) * 0.04) * 4
        }px)`,
        filter: `drop-shadow(0 4px 8px rgba(0, 0, 0, ${
          0.2 + Math.abs(Math.sin((frame - sectionDuration * 3) * 0.04) * 0.1)
        }))`,
      }}
    />
  </Section>
</Sequence>;
```

![Screenshot 2025-06-13 at 5.43.11 PM.png](https://i.postimg.cc/HLmvWRk5/4.png)

Awesome, that’s pretty much it. This is the main thing we’ll be repeating — just keep multiplying the steps by 3. We’ll do the same for PlayArtist too. Genres will be a bit different, but the frontend stays the same — only class names will change here and there. So yeah, go ahead and copy-paste the full code. And if you hit any errors, just Google it or ping us directly.

## [For Full Code Click Here](https://gist.github.com/Shrit1401/e39e1a6745066224e446ae42878f64b1)

The full code is nearly 2000 lines — yeah, wild, I know. In a real production setup, you’d never keep things this chunky. It’s always better to keep it clean and modular — break each section into its own file. That said, I kinda love it this way too. It feels like you’re building something wild from scratch, like chaos turning into something cool. But yeah, totally feel free to split it up however you like.

## Music

let’s have some music, Adding music is super simple. Just use a MP3 file, or use the 5 tracks already in the public/Songs folder. We’ll randomly pick one each time — so your video hits a different vibe on every play. so in the `rewind/page.tsx`

```tsx
export default function RewindPage() {
  const randomSongIndex = Math.floor(Math.random() * 5) + 1;
  ...
  <Player
	  component={SpotifyRewindVideo}
	  durationInFrames={1800}
	  fps={30}
	  compositionWidth={1080}
	  compositionHeight={1920}
	  style={{ width: "100%", borderRadius: "0.75rem" }}
	  controls
	  // add randomSongIndex to inputProps
	  inputProps={{
	    userProfile,
	    topTracks,
	    topArtists,
	    recentlyPlayed,
	    newDiscovery,
	    popularGenres,
	    videoInsights,
	    randomSongIndex,
	  }}
	/>;
```

and in the `SpotifyRewindVideo`

```tsx
import { Audio, staticFile } from "remotion";
...
interface SpotifyRewindVideoProps {
  userProfile: UserProfile;
  topTracks: TopTracksResponse;
  topArtists: TopArtistsResponse;
  recentlyPlayed: RecentlyPlayedResponse;
  newDiscovery: any;
  popularGenres: [string, unknown][];
  videoInsights: VideoInsights;
  randomSongIndex: number;
}
...
export default function SpotifyRewindVideo({
  userProfile,
  topTracks,
  topArtists,
  recentlyPlayed,
  newDiscovery,
  popularGenres,
  videoInsights,
  randomSongIndex,
}: SpotifyRewindVideoProps) {
  const songUrl = staticFile(`songs/song_${randomSongIndex}.mp3`);
...
 <AbsoluteFill style={{ background: "#1DB954" }}>
      <Audio src={staticFile(songUrl)} />
```

Woohoo! Let’s gooo! We’ve officially wrapped up everything related to Development, This is big. Like _really_ big. So when you get there, don’t forget to announce it loud — you’ve done it. That’s wild.

## **Please do this or Shrit will be sad.**

Go ahead and take a screenshot of rewind video. Share it in `#progress` to inspire others who are searching for cool use cases.