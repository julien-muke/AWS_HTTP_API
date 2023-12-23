# 🚨 AWS Project - AWS API Gateway to Lambda Tutorial in Python | Build a HTTP API (2/2)

API Gateway is an AWS service that allows you to build HTTP or REST APIs. In this tutorial, I walk you through how to create a HTTP endpoint, followed by how to create a Lambda function and how to connect the Lambda function to the API Gateway endpoint!


## 🤖 Project Overview

I'm going to show you how to create a http api using api gateway in aws, we're going to be creating two routes for our api the first one is called `/getPerson` and that's going to be a get api and the second one is going to be called `/createPerson` and that's a post api. 

These are going to be backed by a single lambda function and i'm going to add some conditional logic in this lambda function so that it can handle both of these different endpoints.


## 📋 The Application Architecture

![Screenshot](/img/aws_archi.png)

## 👉 Step 1: Creating the HTTP API in the API Gateway

Go to the console and navigate to api gateway, choose HTTP API then click build


![Screenshot](/img/create_http_api.png)


Click on add integration, the drop down lets us specify what we want to do, we want to select
lambda for our integration since we're going to create a lambda function now it's asking us for some of the
details of that lambda function it's asking us for the aws region that it's located in and the name of that lambda function.

But before that let's create a lambda function first then we will come back to select the lambda function


![Screenshot](/img/create_api.png)


Go to the search bar and search for lambda, go to the top right and click on create function


![Screenshot](/img/create_lambda.png)


Author from scratch is a good option to use, we're going to leave pretty much everything default leave put in whatever name you want so mine was just called CrudAppFunction function and we are going to be using python 3.8 so make sure you pick python 3.8 leave everything else defaults and click create function


![Screenshot](/img/create_function.png)


We're going to doing do some code editing a little bit later in the console, directly in the editor if we click on `lambda_function.py` that is what is currently in this lambda function, it's just a lambda handler it's returning hello from lambda, we're going to keep for now so what i want to do now and come back to that later


![Screenshot](/img/lambda_function.png)



Go back the api gateway refresh the page to update the changes, go ahead and click on lambda functions and now you will see our lambda function popping up click on it and for api name we can pick whatever you want, i'm just going to call mine persons and click on the next button to go to the next section


![Screenshot](/img/api.png)


The next section is about routes we said that we wanted to create two different routes for this exercise one was a `GET` for `/getPerson` route and the other was a `POST` for `/createPerson`, change the method to get and the the resource path is going to be slash get person and then the target is going to be what we just integrated with which is our CrudAppFunction.

We also need to create a second route which is a post and that is going to be slash create person and it's also going to point to the same lambda function, because if you wanted to create two different functions one for each nothing wrong with that but i'm just going to do this in a way such that in my code itself i'll know whether or not get person or create person isn't being invoked so i can respond respectively so that's all we really need to do here click on next



![Screenshot](/img/route.png)



The next step is for stages if you want to use a stage that means something maybe test, beta or production depending on what you're trying to do you can go and name it whatever you want or leave it as default, i'm going to leave it as default in my case.

The second option is auto deploy and it basically just allows you to say if i change any settings for this HTTP API automatically deploy a new version to respect those settings if you don't like that or you think it's too dangerous you can disable that and then you'll have to manually deploy your API every time this is more similar to what you have in the REST version of API Gateway but i'm going to leave it on and click on next. Review everything and click on create


![Screenshot](/img/stage.png)


To test our API we're going to use a tool called Postman which is a tool that helps you make service calls or requests to endpoints and it allows you to configure a whole bunch of options along the way, we'll use Postman test both the `/getPerson` and the `/createPerson` API before we actually write our code. So back to the API Gateway and copy the `Invoke URL`



![Screenshot](/img/invoke_url.png)



Open Postman i'm going to open two tabs on the left, the one that i'm on right now is going to be for `/getPerson` and if i just test this first i just click on send we're  calling that API Gateway endpoint and then turn the lambda function whit the output `"Hello from lambda"`.

I will add some parameters so if i were creating a get person API the most logical thing that a person would pass into this API is probably something like `personId` as a Key and `personId123` as a value



![Screenshot](/img/postman_test_1.png)



Let's go ahead and set up the other API which is `/createPerson` so let's do the same thing other tab now i'm going to change it to POST now and the same kind of exercise applies we need to put some data in the the body of this request since it's a POST
click on body instead of params i'm going to change this to raw just so
that it's in JSON, let's say in this there's probably a million other ways to do this by the way so i'm probably doing to creating a person something like first name and then let's call it julien and let's say last name Muke and let's say email myemail gmail.com



![Screenshot](/img/postman_2.png)


Go back the lambda function, just early on the the goal of our next step is to figure out do i extract values
from my events because the values that get passed into the lambda function from API Gateway based on what we just did in Postman are going to be inside the event object.

Copy and paste the code to the `lambda_function.py` make sure you click deploy button


```bash
import json
import uuid

GET_RAW_PATH = "/getPerson"
CREATE_RAW_PATH = "/createPerson"

def lambda_handler(event, context):
    print(event)
    if event['rawPath'] == GET_RAW_PATH:
        print('Received getPerson request')
        personId = event['queryStringParameters']['personId']
        print("with param personId=" + personId)
        return { "firstName": "Julien " + personId, "lastName": "M", "email": "myEmail@gmail.com" }
    elif event['rawPath'] == CREATE_RAW_PATH:
        print('Received createPerson request')
        decodedBody = json.loads(event['body'])
        firstname = decodedBody['firstname']
        print('with param firstname=' + firstname)
        return { "personId": str(uuid.uuid1())}

```


![Screenshot](/img/lambda_code.png)



Let's go back to Postman and invoke this we're just going to get a response back but if we click on send we should see some kind of person id key and a random ID being returned back to us



![Screenshot](/img/postman_3.png)
























## 📋 <a name="table">Table of Contents</a>

1. 🤖 [Introduction](#introduction)
2. ⚙️ [Tech Stack](#tech-stack)
3. 🔋 [Features](#features)
4. 🤸 [Quick Start](#quick-start)
5. 🕸️ [Snippets](#snippets)
6. 🔗 [Links](#links)
7. 🚀 [More](#more)

## 🚨 Tutorial

This repository contains the code corresponding to an in-depth tutorial available on our YouTube channel, <a href="https://www.youtube.com/@javascriptmastery/videos" target="_blank"><b>JavaScript Mastery</b></a>. 

If you prefer visual learning, this is the perfect resource for you. Follow our tutorial to learn how to build projects like these step-by-step in a beginner-friendly manner!

<a href="https://youtu.be/zgGhzuBZOQg" target="_blank"><img src="https://github.com/sujatagunale/EasyRead/assets/151519281/1736fca5-a031-4854-8c09-bc110e3bc16d" /></a>

## <a name="introduction">🤖 Introduction</a>

Built on Next.js 14, the events application stands as a comprehensive, full-stack platform for managing events. It serves as a hub, spotlighting diverse events taking place globally. Featuring seamless payment processing through Stripe, you have the capability to purchase tickets for any event or even initiate and manage your own events.

If you're getting started and need assistance or face any bugs, join our active Discord community with over 27k+ members. It's a place where people help each other out.

<a href="https://discord.com/invite/n6EdbFJ" target="_blank"><img src="https://github.com/sujatagunale/EasyRead/assets/151519281/618f4872-1e10-42da-8213-1d69e486d02e" /></a>

## <a name="tech-stack">⚙️ Tech Stack</a>

- Node.js
- Next.js
- TypeScript
- TailwindCSS
- Stripe
- Zod
- React Hook Form
- Shadcn
- uploadthing

## <a name="features">🔋 Features</a>

👉 **Authentication (CRUD) with Clerk:** User management through Clerk, ensuring secure and efficient authentication.

👉 **Events (CRUD):** Comprehensive functionality for creating, reading, updating, and deleting events, giving users full control over event management.
- **Create Events:** Users can effortlessly generate new events, providing essential details such as title, date, location, and any additional information.
- **Read Events:** Seamless access to a detailed view of all events, allowing users to explore event specifics, including descriptions, schedules, and related information.
- **Update Events:** Empowering users to modify event details dynamically, ensuring that event information remains accurate and up-to-date.
- **Delete Events:** A straightforward process for removing events from the system, giving administrators the ability to manage and curate the platform effectively.
        
👉 **Related Events:** Smartly connects events that are related and displaying on the event details page, making it more engaging for users
    
👉 **Organized Events:** Efficient organization of events, ensuring a structured and user-friendly display for the audience, i.e., showing events created by the user on the user profile
    
👉 **Search & Filter:** Empowering users with a robust search and filter system, enabling them to easily find the events that match their preferences.
    
👉 **New Category:** Dynamic categorization allows for the seamless addition of new event categories, keeping your platform adaptable.
    
👉 **Checkout and Pay with Stripe:** Smooth and secure payment transactions using Stripe, enhancing user experience during the checkout process.
    
👉 **Event Orders:** Comprehensive order management system, providing a clear overview of all event-related transactions.
    
👉 **Search Orders:** Quick and efficient search functionality for orders, facilitating easy tracking and management.

and many more, including code architecture and reusability 

## <a name="quick-start">🤸 Quick Start</a>

Follow these steps to set up the project locally on your machine.

**Prerequisites**

Make sure you have the following installed on your machine:

- [Git](https://git-scm.com/)
- [Node.js](https://nodejs.org/en)
- [npm](https://www.npmjs.com/) (Node Package Manager)

**Cloning the Repository**

```bash
git clone https://github.com/your-username/your-project.git
cd your-project
```

**Installation**

Install the project dependencies using npm:

```bash
npm install
```

**Set Up Environment Variables**

Create a new file named `.env` in the root of your project and add the following content:

```env
#NEXT
NEXT_PUBLIC_SERVER_URL=

#CLERK
NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY=
CLERK_SECRET_KEY=
NEXT_CLERK_WEBHOOK_SECRET=

NEXT_PUBLIC_CLERK_SIGN_IN_URL=/sign-in
NEXT_PUBLIC_CLERK_SIGN_UP_URL=/sign-up
NEXT_PUBLIC_CLERK_AFTER_SIGN_IN_URL=/
NEXT_PUBLIC_CLERK_AFTER_SIGN_UP_URL=/

#MONGODB
MONGODB_URI=

#UPLOADTHING
UPLOADTHING_SECRET=
UPLOADTHING_APP_ID=

#STRIPE
STRIPE_SECRET_KEY=
STRIPE_WEBHOOK_SECRET=
NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY=
```

Replace the placeholder values with your actual credentials 

**Running the Project**

```bash
npm start
```

Open [http://localhost:3000](http://localhost:3000) in your browser to view the project.


## <a name="more">🚀 More</a>

**Advance your skills with Next.js 14 Pro Course**

Enjoyed creating this project? Dive deeper into our PRO courses for a richer learning adventure. They're packed with detailed explanations, cool features, and exercises to boost your skills. Give it a go!

<a href="https://jsmastery.pro/next14" target="_blank">
<img src="https://github.com/sujatagunale/EasyRead/assets/151519281/557837ce-f612-4530-ab24-189e75133c71" alt="Project Banner">
</a>

<br />
<br />


#