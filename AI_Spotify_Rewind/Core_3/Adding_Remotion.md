Alright, now let's make some videos. So, for videos, we are going to use something known as [Remotion](https://www.remotion.dev/)

Awesome! So now, we are going to set up remotion. We already have packages installed. You can see it the `package.json` .

Alright, so in this section, we are not really going to make videos, but we are going to set up some use states which will help us to make the video in next section. So let's get started. The first thing which we'll do is to add a generate button in our `rewind page`.

```tsx
						...
						<div className="button-container flex justify-center my-8">
              <button
                type="button"
                className="button bg-green-600 hover:bg-green-700 text-white font-bold py-3 px-8 rounded-full text-lg transition-all"
              >
                Generate My Rewind
              </button>
            </div>
           	... 
```

![Screenshot 2025-06-11 at 2.29.29 PM.png](https://i.postimg.cc/XY8SkHw3/image2.png)

looks so cool, let’s add some usestates

```tsx
export default function RewindPage() {
	...
	const [btnLoading, setBtnLoading] = useState(false);
	//we're gonna use this to store ai generated response
  const [videoInsights, setVideoInsights] = useState<VideoInsights>();
  
  const handleClick = async () => {
    setBtnLoading(true);
    alert("clicked");
    setBtnLoading(false);
  };
  ...
  <div className="button-container flex justify-center my-8">
	  <button
	    onClick={handleClick}
	    disabled={btnLoading}
	    type="button"
	    className="button bg-green-600 hover:bg-green-700 text-white font-bold py-3 px-8 rounded-full text-lg transition-all"
	  >
	    {btnLoading ? "Generating..." : "Generate My Rewind"}
	  </button>
	</div>;

	....
```

cool now whenever you click you see an alert saying clicked, no let’s hook it to our `generateVideoFacts()` function

```tsx
import { generateVideoFacts } from "@/lib/gemini";

export default function RewindPage() {
	...
	const handleClick = async () => {
    setBtnLoading(true);
    // whenever we add ! we are saying that the value is not null
    const videoInsights = await generateVideoFacts(
      //whenever we add ! it's    
      userProfile!,
      topTracks!,
      topArtists!,
      popularGenres!,
      recentlyPlayed!,
      newDiscovery!
    );

    if (!videoInsights) {
      console.error("Error generating video insights");
      setBtnLoading(false);
      return;
    }

    console.log(videoInsights);

    setVideoInsights(videoInsights);
    setBtnLoading(false);
  };
  ...
```

awesome now reload the page, click the button and check your console

![Screenshot 2025-06-11 at 6.02.49 PM.png](https://i.postimg.cc/3RNLp9ZL/2.png)

let’s go lol I am embarrassed but  kinda shocked Madira was literally I was listening few minutes back

(btw remove the code to log `videoInsights` )

now let’s show a video, for now it’ll be a sample

## Showing Video

just below the loading state write this code in `rewind.tsx`

```tsx
	 ...
   )}
	   // having so much checks ensures that video plays whenever everything is loaded
      <div className="remotion-player">
        {videoInsights &&
          userProfile &&
          topTracks &&
          topArtists &&
          newDiscovery &&
          recentlyPlayed &&
          popularGenres && (
            <div className="h-[1920px] w-[1080px] flex flex-col items-center justify-center">
              <h1>Hello</h1>
            </div>
          )}
      </div>
    </>
  );
}
```

awesome one more thing i want that whenever we click button and it’s loaded it automaticlly goes their so we can use a `useeffect` for this

```tsx
useEffect(() => {
    if (videoInsights) {
      const videoElement = document.querySelector(".remotion-player");
      if (videoElement) {
        setTimeout(() => {
          videoElement.scrollIntoView({ behavior: "smooth" });
        }, 200); // small delay ensures element is rendered
      }
    }
  }, [videoInsights]);
```

add this line on the top this will make our scrolling 100x better

now click the button, and it should scroll

And that's it. Right now, I believe you are doing some heavy work, and in the later sections, it's going to be more heavy, honestly. But whenever you feel like stuck, I will say it again, talk to me, talk to people on the Discord. It's fairly new community, so I can give more attention to you, and I am always glued to my PC, so I will definitely see your message.

## **Please do this or Shrit will be sad.**

Go ahead and take a screenshot of rewind page. Share it in `#progress` to inspire others who are searching for cool use cases.