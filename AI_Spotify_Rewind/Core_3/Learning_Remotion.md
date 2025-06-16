## Player

just like every video needs a player remotion also gives us something like a Player, we can simiply add it in `rewind/page.tsx` with a `<Player/>`

```tsx
import { Player } from "@remotion/player";
...
{
  videoInsights &&
    userProfile &&
    topTracks &&
    topArtists &&
    newDiscovery &&
    recentlyPlayed &&
    popularGenres && (
      <div className="remotion-player w-[1920px] h-[1080px]">
        <div className="mx-auto max-w-md overflow-hidden rounded-xl shadow-lg ">
          <Player
	          // you'll get error here
            component={}
            durationInFrames={1800}
            fps={30}
            compositionWidth={1080}
            compositionHeight={1920}
            style={{ width: "100%", borderRadius: "0.75rem" }}
            controls
            inputProps={{
              userProfile,
              topTracks,
              topArtists,
              recentlyPlayed,
              newDiscovery,
              popularGenres,
              videoInsights,
            }}
          />
        </div>
      </div>
    );
}

```

you will be seeing an error saying `JSX attributes must only be assigned a non-empty 'expression'.`

let’s fix this error

## Video

alright we need a video to play, inside the `components` folder create a file as `SpotifyRewindVideo.tsx` and write a boiler code as

```tsx
import React from "react";

const SpotifyRewindVideo = () => {
  return <div>Hello</div>;
};

export default SpotifyRewindVideo;
```

cool now hook it up to the `rewind/page.tsx`

```tsx
import SpotifyRewindVideo from "@/components/SpotifyRewindVideo";
...
{
  videoInsights &&
    userProfile &&
    topTracks &&
    topArtists &&
    newDiscovery &&
    recentlyPlayed &&
    popularGenres && (
      <div className="remotion-player w-[1920px] h-[1080px]">
        <div className="mx-auto max-w-md overflow-hidden rounded-xl shadow-lg ">
          <Player
            component={SpotifyRewindVideo}
            durationInFrames={1800}
            fps={30}
            compositionWidth={1080}
            compositionHeight={1920}
            style={{ width: "100%", borderRadius: "0.75rem" }}
            controls
            inputProps={{
              userProfile,
              topTracks,
              topArtists,
              recentlyPlayed,
              newDiscovery,
              popularGenres,
              videoInsights,
            }}
          />
        </div>
      </div>
    );
}
```

now if you go you’ll see a simple hello written in there

![Screenshot 2025-06-13 at 12.29.35 PM.png](https://i.postimg.cc/zB1zGtYg/1.png)

something like this awesome we have something on our player, now let’s make it beautiful

## Absolute Fill And Live Background

absolute fill is basically the whole screen, so we can apply backgrounds to it, something like this

```tsx
import { AbsoluteFill } from "remotion";
import React from "react";

const SpotifyRewindVideo = () => {
  return (
    <AbsoluteFill style={{ background: "#1DB954" }}>
      <div>Hello</div>
    </AbsoluteFill>
  );
};

export default SpotifyRewindVideo;
```

![Screenshot 2025-06-13 at 1.00.51 PM.png](https://i.postimg.cc/C5cSJwBG/2.png)

here you go, woho now we have a green background, but now i wanna take it to one step further with moving background, if you see closely then we have a file called as `AuroraBackground.tsx` inside `components` , it basically a bg provided by [aceternity UI](https://ui.aceternity.com/), we can do this with something like

```tsx
..
import { AuroraBackground } from "./AuroraBackground";

const SpotifyRewindVideo = () => {
  return (
    <AbsoluteFill style={{ background: "#1DB954" }}>
      <AuroraBackground
        style={{ position: "absolute", width: "100%", height: "100%" }}
      >
        <div style={{ width: "100%", height: "100%" }}></div>
      </AuroraBackground>
      <div>Hello</div>
    </AbsoluteFill>
  );
};

export default SpotifyRewindVideo;
```

![Screenshot 2025-06-13 at 1.03.18 PM.png](https://i.postimg.cc/7YLxhJdm/3.png)

so cool now we have a background which actually moves sheeesh

## Sequence

Alright, just like when you edit a video in Premiere Pro — you know, you add small clips one after another to build the full video — in Remotion (or DreamOcean), we do something similar using something called a **Sequence**.

Think of it like this:

The first sequence could be your intro, the next one might show your top songs, the next one your favorite artist, and so on. Each Sequence is like a mini scene, and they play one after the other.

So yeah, if you add 2–3 sequences, they’ll all play step by step — first, second, third, fourth — like a timeline of clips in your editing tool. Super simple, but super powerful.

let’s have a simple sequence in our app

```tsx
import { AbsoluteFill, Sequence } from "remotion";
...
<AbsoluteFill style={{ background: "#1DB954" }}>
  <AuroraBackground
    style={{ position: "absolute", width: "100%", height: "100%" }}
  >
    <div style={{ width: "100%", height: "100%" }}></div>
  </AuroraBackground>

  {/* whenever we write a sequence, we need to give it a from and durationInFrames */}
  <Sequence from={0} durationInFrames={30}>
    one
  </Sequence>
  <Sequence from={30} durationInFrames={30}>
    two
  </Sequence>
  <Sequence from={60} durationInFrames={30}>
    three
  </Sequence>
</AbsoluteFill>;
```

now if you go back you’ll see one two three in the application

## Styled Components

Time to add some ✨ style ✨ to spice things up.

**First way** is inline styling — like how we did it in the AbsoluteFill, where we styled it directly with a background colour or gradient

AbsoluteFill, where we styled it directly with a background color or gradient, let’s see how to do it

```tsx
import styled from "@emotion/styled";

// we can use styled components to style the container
const CenteredContainer = styled.div`
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: 100%;
`;
...
<Sequence from={0} durationInFrames={30}>
  <CenteredContainer>
    <div style={{ fontSize: "72px", fontWeight: "bold", color: "black" }}>
      one
    </div>
  </CenteredContainer>
</Sequence>
<Sequence from={30} durationInFrames={30}>
  <CenteredContainer>
    <div style={{ fontSize: "72px", fontWeight: "bold", color: "black" }}>
      two
    </div>
  </CenteredContainer>
</Sequence>
<Sequence from={60} durationInFrames={30}>
  <CenteredContainer>
    <div style={{ fontSize: "72px", fontWeight: "bold", color: "black" }}>
      three
    </div>
  </CenteredContainer>
</Sequence>
```

with this simple thing now the text will be bold and little bigger

![Screenshot 2025-06-13 at 1.20.17 PM.png](https://i.postimg.cc/3RCK4X3b/4.png)

awesome

## Animations

Animations in ReMotion can be a bit tricky. While they’re an important part of making things look good, ReMotion doesn’t give you simple built-in tools for them—you actually have to create the animations yourself using math. Things like scaling, opacity, and position all need to be calculated manually.

Personally in while coding, I didn’t write all the animations by hand. I just used ChatGPT to help generate the animation code

That said, for the sake of learning and testing, let’s try creating a few animations ourselves. What I did in my project was create a helper function called animatedText, which I used for all the text animations across sequences. This made the code much cleaner and easier to manage.

full code

```tsx
import {spring, useCurrentFrame } from "remotion";
...
// helper function to animate the text
const AnimatedText = ({ text, delay }: { text: string; delay: number }) => {
  // useCurrentFrame is a hook that returns the current frame of the video
  const frame = useCurrentFrame();

  // spring is a function that animates the text, in this case, we are animating the scale of the text
  const scale = spring({
    // frame is the current frame of the video
    frame: frame - delay,
    // fps is the frames per second of the video
    fps: 30,
    // from is the starting value of the animation
    from: 0,
    // to is the ending value of the animation
    to: 1,
    // config is the configuration of the animation
    config: {
      // damping is the amount of damping of the animation
      damping: 12,
      // mass is the mass of the animation
      mass: 0.5,
    },
  });

  // opacity is the opacity of the text
  const opacity = spring({
    frame: frame - delay,
    fps: 30,
    from: 0,
    to: 1,
    config: {
      // damping is the amount of damping of the animation
      damping: 12,
      // mass is the mass of the animation
      mass: 0.5,
    },
  });

  return (
    <div
      style={{
        fontSize: "72px",
        fontWeight: "bold",
        color: "black",
        transform: `scale(${scale})`,
        opacity,
      }}
    >
      {text}
    </div>
  );
};
...
<Sequence from={0} durationInFrames={30}>
  <CenteredContainer>
    <AnimatedText 
      text="one" 
      delay={0}
    />
  </CenteredContainer>
</Sequence>
<Sequence from={30} durationInFrames={30}>
  <CenteredContainer>
    <AnimatedText 
      text="two" 
      delay={0}
    />
  </CenteredContainer>
</Sequence>
<Sequence from={60} durationInFrames={30}>
  <CenteredContainer>
    <AnimatedText 
      text="three" 
      delay={0}
    />
  </CenteredContainer>
</Sequence>
```

Awesome! Now we have actually learned to make videos. In the next section we are going to make a whole video. It would be more of a copy-paste session than writing code, but lesgo!

## **Please do this or Shrit will be sad.**

Go ahead and take a screenshot of rewind video. Share it in `#progress` to inspire others who are searching for cool use cases.