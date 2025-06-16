Let’s start something!

in our first core we’re gonna start with setting up with an idea and then using spotify api to get the stats

## Before we Begin
[this is the spotify rewind which I created](https://www.loom.com/share/5a6901873b434314b8eeae00b32b2f06?sid=c74a2573-606a-4d91-b6bd-93c41934fd0f), if you see then I am not just making a basic spotify rewind, instead I am trying to create a _**rude spotify rewind**_ and I wanna encourage you to have something like this, spotify rewind is cool and all but that’s what spotify does every year what’re you doing to make it unique?

I want everyone to know what unique you’ll be adding to your rewind, some ideas are

- **Boomer Rewind**: _“Your Spotify, as judged by a confused 60-year-old who misses ‘real music’.”_
- **Therapist Rewind**: _“An unsolicited therapy session disguised as your Spotify stats.”_
- **Villain Arc Rewind**: _“Your year in music, reimagined as the rise of a beautifully broken supervillain.”_

These are some ideas make sure you think of something on your own and go forward with it

**this is a very important step please don’t copy paste my idea, else you’ll not have fun 👿👿**

## Clone the repo

for us to save time + avoid mismatch of packages we’re gonna use a starter to make our things easier

We’ve created a project with a basic React/NextJS project + some CSS to keep you moving fast. Go ahead and fork the project [here](https://github.com/Forge-Zone/spotify-rewind-starter).

To do so, just click “**Fork**” at the top and then clone the forked repo. This will fork the repo into your personal account which will make it easier to push code/deploy our project later.

Also, drop a star on it as well if you’re feeling nice!

![image.png](https://i.postimg.cc/HW3XTJhW/image2.png)

Once you’ve forked the repo, go ahead and clone it, your clone URL will be specific to you. if you have no idea how to download the repo to your desktop here’s a little walkthrough video

alright now you have starter code on desktop open it in your code editor,

let’s download all the packages and then run the project locally

```jsx
# install packages
npm install 

# run it
npm run dev
```

Then, head to [localhost:3000](http://localhost:3000/) and you should see the following:

![image.png](https://i.postimg.cc/qBLKLtcV/image3.png)

**A note for those _not that familiar_ with React** — You don’t need to be a React expert to do this. If there are some things you don’t understand as you go through this, just Google them and power through.

## Your Idea

alright I hope you would’ve picked your idea, now let’s have your idea in the landing page

go to `app/page.tsx` and change your title and subtitle

remember:

It’s just like a landing page! The headline is short and sweet. And, the subtitle explains a bit more. (use AI for this, it’ll be helpful)

```jsx
const Home = () => {
  return (
    <div className="container">
      <div className="header">
        <div className="header-title">
          {/* your headline here */}
          <h1>Rude Spotify Rewind</h1>
        </div>
        <div className="header-subtitle">
          {/* your subtitle here */}
          <h2>Exposing your terrible music taste with zero mercy</h2>
        </div>
      </div>
    </div>
  );
};
```

![image.png](https://i.postimg.cc/4xKhsBDs/image4.png)

Please **don’t copy** any of my headlines here — come up with your own specific use-case. Add your own spin to this thing. Otherwise, it won’t be fun I promise.

## Add Login Button

I have provided a nice design system in `global.css` you can read through it we’re gonna use them to have a nice button

this button will be used to login our spotify

first import link this is a like `a` component in html with few advancements

```jsx
 import Link from "next/link";
```

then

```jsx
import Link from "next/link";

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

        {/* write code from here*/}
        <div className="button-container">
          <Link href="#">
            <button className="button">Login with Spotify</button>
          </Link>
        </div>
      </div>
    </div>
  );
};
```

![image.png](https://i.postimg.cc/289BsPX0/image5.png)

awesome start let’s move forward 🙂

## **Please do this or Shrit will be sad.**

Go ahead and take a screenshot of your Spotify Rewind with your fancy new headline and subtitle. Share it in `#progress` to inspire others who are searching for cool use cases.
