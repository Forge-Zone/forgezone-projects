i feel like best way to learn is to mess around, more you mess around more you explore territory you have never seen

we’ve just added AI, let’s see what different things gemini offer us

*btw, this part’s totally optional — feel free to skip ahead. but I have a feeling messing around here might be pretty fun for you

inside the `lib` folder create `gemini.ts`

```jsx
import { GoogleGenAI } from "@google/genai";

const apiKey = process.env.NEXT_PUBLIC_GOOGLE_GEMINI_API_KEY;
const ai_model = "gemini-2.0-flash";
const ai = new GoogleGenAI({ apiKey });
```

using gemini package we’re creating an ai object, feel free to choose any ai model, [here is list of all gemini models](https://ai.google.dev/gemini-api/docs/models)

alright let’s make our ai generate images (make sure to use a model which supports images, i am using `gemini-2.0-flash-preview-image-generation` for image gen so now in the `gemini.ts`

```jsx
import { GoogleGenAI, Modality } from "@google/genai";
...
const ai_model = "gemini-2.0-flash-preview-image-generation";
...

export const generateImage = async (prompt: string) => {
  try {
    const response = await ai.models.generateContent({
      model: ai_model,
      contents: prompt,
      config: {
	      //modality just tells us which 
        responseModalities: [Modality.TEXT, Modality.IMAGE],
      },
    });

    console.log("Response received:", response);
};

```

cool this generates very cool images, now let’s go to our frontend and write some code to make it work

in the `rewind/page.tsx`

```jsx
previous code....
<div className="container max-w-7xl mx-auto px-4">
  <div className="header">
    <div className="header-title mb-6">
      <h1 className="text-4xl font-bold">Hello {userProfile?.display_name}</h1>
    </div>
    <div className="flex flex-col gap-4">
      <div className="prompt-container">
        <div className="prompt-box">
          <textarea
            placeholder="Enter prompt to generate image..."
            className="w-full h-full bg-transparent border-none outline-none resize-none text-[var(--text-color)] placeholder-[var(--text-muted)]"
          />
        </div>
      </div>
      <div className="button-container">
        <button className="button">Generate Image</button>
      </div>
    </div>
  </div>
</div>;
```

we already some styling for you so it makes frontend very easy for you

![Screenshot 2025-06-10 at 4.31.04 PM.png](https://i.postimg.cc/Y0W9Vfb8/1.png)

awesome not let’s hook to some usestates and make it actually work

```jsx
export default function RewindPage() {
	....
  const [prompt, setPrompt] = useState("");
  const [generatedImage, setGeneratedImage] = useState<string | null>(null);
	....
	
  const handleGenerateImage = async () => {
      const res = await generateImage(prompt);
      console.log(res);
  };
  
  return (
			  ....
				<div className="flex flex-col gap-4">
			  <div className="prompt-container">
			    <div className="prompt-box">
			      <textarea
			        value={prompt}
			        onChange={(e) => setPrompt(e.target.value)}
			        placeholder="Enter prompt to generate image..."
			        className="w-full h-full bg-transparent border-none outline-none resize-none text-[var(--text-color)] placeholder-[var(--text-muted)]"
			      />
			    </div>
			  </div>
			....
			
```

btw `...` means rest of the code

awesome now let’s generate, my prompt was `ppl flying`

![Screenshot 2025-06-10 at 4.36.11 PM.png](https://i.postimg.cc/9zHMjRk2/2.png)

in a moment if everything works fine you will able to see this in the console

![Screenshot 2025-06-10 at 4.37.00 PM.png](https://i.postimg.cc/zfwPwVWQ/4.png)

you can see that in that we have two data first is our text and second in image type, if you don’t know then the data property inside the inlinedata is a base64 type image if we get this we can directly show this in our frontend

```jsx

export const generateImage = async (prompt: string) => {
  try {
    const response = await ai.models.generateContent({
      model: ai_model,
      contents: prompt,
      config: {
        // modalities are the types of responses you want to get back
        responseModalities: [Modality.TEXT, Modality.IMAGE],
      },
    });

    console.log("Response received:", response);
		
    // The image data is in the second part of the response
    const imagePart = response.candidates?.[0]?.content?.parts?.[1];
    if (imagePart?.inlineData?.data) {
      return imagePart.inlineData.data;
    }
    console.log("No image data found in response");
    return null;
  } catch (error) {
    console.error("Error generating image:", error);
    return null;
  }
};
```

here we are returning the imagedata from the image i shown you not it’s super simple

```jsx
					const handleGenerateImage = async () => {
					    try {
					      const imageData = await generateImage(prompt);
					
					      if (imageData) {
						      // the image url is how we pass it 
					        const imageUrl = `data:image/jpeg;base64,${imageData}`;
					        setGeneratedImage(imageUrl);
					      } else {
					        console.log("No image data received");
					      }
					    } finally {
					    }
					  };
					....
              <div className="button-container">
                <button onClick={handleGenerateImage} className="button">
                  Generate Image
                </button>
              </div>
              ....
              {generatedImage && (
                <div className="mt-4">
                  <img
                    src={generatedImage}
                    alt="Generated image"
                    className="max-w-full h-auto rounded-lg shadow-lg"
                  />
                </div>
              )}
              ....
            </div>
```

awesome now if everything is fine you can go to the website and will have something like this

![Screenshot 2025-06-10 at 4.46.03 PM.png](https://i.postimg.cc/pLYC93KQ/3.png)

awesome we can now generate images using gemini

## Show

even though this is not we’re building but, I think this is cool go to your wife/sis/friend and show it, you can’t deploy yet but you can show from your system, take their feedback see them having fun with it, this way you’re going to be a builder and I highly suggest you [tweet this](https://ctt.ac/eCe4z) :)

I am gonna delete everything related to image gen because we’re making spotify rewind clone, but go ahead export this page to a new app you can even change few things, make something like

- a yt thumbnail maker
- wallpaper maker
- Logo maker

etc these are examples feel free to do something cool :)

## **Please do this or Shrit will be sad.**

Go ahead and take a screenshot of your best image generated with your display name. Share it in `#progress` to inspire others who are searching for cool use cases.