# Server of Comer project

This document outlines the server configuration for Lantana (GaYeong)'s project.

**Deployed Server URL**: [Comer Web Service](https://comer-experience-app-server.onrender.com)
**Deployed Client URL**: [Comer Static Site](https://comer-app.onrender.com/)

## Overview

- **Name**: server
- **Version**: 1.0.0
- **Main Entry**: index.js
- **Author**: Lantana (GaYeong)

Backend: Node.js, Express.js, Mongoose, MongoDB (NoSQL)

# Project Dependencies

- **[@aws-sdk/client-s3](https://www.npmjs.com/package/@aws-sdk/client-s3)**: AWS SDK client for S3, used for integrating with AWS S3 services for image file storage.
- **[@aws-sdk/credential-provider-env](https://www.npmjs.com/package/@aws-sdk/credential-provider-env)**: AWS SDK credential provider for environment variables.
- **[@aws-sdk/lib-storage](https://www.npmjs.com/package/@aws-sdk/lib-storage)**: AWS SDK library for storage services.
- **[@aws-sdk/s3-request-presigner](https://www.npmjs.com/package/@aws-sdk/s3-request-presigner)**: AWS SDK library for generating pre-signed S3 requests.
- **[bcryptjs](https://www.npmjs.com/package/bcryptjs)**: Library for hashing passwords securely.
- **[body-parser](https://www.npmjs.com/package/body-parser)**: Middleware for parsing incoming request bodies in Express.
- **[cookie-parser](https://www.npmjs.com/package/cookie-parser)**: Middleware for parsing cookies in Express.
- **[cors](https://www.npmjs.com/package/cors)**: Middleware for enabling CORS (Cross-Origin Resource Sharing) in Express.
- **[date-fns](https://www.npmjs.com/package/date-fns)**: JavaScript date utility library for handling dates and times.
- **[dotenv](https://www.npmjs.com/package/dotenv)**: Library for loading environment variables from a .env file into process.env.
- **[express](https://www.npmjs.com/package/express)**: Fast, unopinionated, minimalist web framework for Node.js, used as the core framework for the server.
- **[jsonwebtoken](https://www.npmjs.com/package/jsonwebtoken)**: Library for generating and verifying JSON Web Tokens (Refresh and access Tokens) for authentication.
- **[mongoose](https://www.npmjs.com/package/mongoose)**: MongoDB object modeling tool designed to work in an asynchronous environment.
- **[multer](https://www.npmjs.com/package/multer)**: Middleware for handling file uploads in Express.
- **[multer-s3](https://www.npmjs.com/package/multer-s3)**: Multer storage engine for uploading files directly to AWS S3.
- **[nodemailer](https://www.npmjs.com/package/nodemailer)**: Library for sending emails with Node.js.

## Getting Started

To get started with this server, clone the repository and install dependencies:

```bash
git clone [https://github.com/gayeongpark/comer_server.git]
cd comer_server
npm install
```

Run the server:

```bash
node index.js
```

## User Case (Entities)

1. **Users:**

   **Account Creation and Verification:**
   Users have the capability to create an account by providing an email address, setting a password, and confirming it. Upon submission, they receive an email containing a verification token. Clicking on the URL within the email activates the account, enabling access to various features on the website.

   **Profile Management:**
   Once registered, users can update their profiles and view booked experiences. They can also access previously created culinary events.

   **Password Management:**
   Users have the option to reset their passwords. If forgotten, they can initiate the process by providing their email address. Upon confirmation, a new email containing a verification token is sent. Clicking on the URL in the email allows users to reset their password. The new password must not match the previous one.

   **Account Deletion:**
   Users can choose to delete their accounts. This action changes their status to inactive without deleting their entire dataset.

2. **Experiences:**

   **Event Creation and Editing:**
   Users have the ability to create and edit multiple events. They can upload image files, which are stored on an AWS S3 bucket. Event details encompass various aspects such as title, description, perks (e.g., food, beverages, transportation, equipment), guest requirements (minimum age, allowance of kids and pets, maximum group size), languages spoken, general availability (start time, end time, date range), tags, price with currency, additional notes for user consideration, likes from other users, and cancellation policy. These details are stored in the database.

   **Event Location:**
   To aid in locating events, relevant information including country, city, state, address (including street number), longitude, latitude, and full address is stored in the database. The location details are retrieved via the Mapbox library from the client side.

   **Display Event:**
   Users can view all saved experiences on the main page. The server retrieves events that are not dated before today's date, ensuring relevance and timeliness of displayed content. Additionally, on the event detail page, available dates and times will be listed. Dates displayed will not precede today's date, maintaining accuracy and relevance.

   **Event Editing Process:**
   When editing events, users can modify specific information without the need to input all details again.

   **Event Search:**
   Users have the capability to search for events by city name, enhancing the accessibility and user-friendliness of the platform.

3. **Comments:**

   Users can create and delete multiple comments on the detailed event pages. Each experience page may host multiple comments. When a comment is posted, it is stored in the database along with the user ID and experience ID as foreign keys, along with the comment's description.

4. **Availabilities:**

   **Entity Creation:**
   When a user opens their event, an availability entity is generated utilizing data from the start time, end time, and date range of the Experience entity. This entity includes the experience ID as a foreign key, multiple dateMaxGuestPairs, many booking details (if a user booked a slot) to track event bookings, and a timestamp.

   **DateMaxGuestPairs Details:**
   Within the indexed dateMaxGuestPairs, objects are stored, containing details such as the date, start time, end time, maximum number of guests allowed, price, and current status.

   **Booking Process:**
   Upon a user booking the event, the maximum number of guests allowed is appropriately decreased to the relevant data and time slot.

   **Booking Object Creation:**
   Additionally, one a user book a slot from an event, an object is created within the availabilities.booking array. This object includes details like date, slotId, userId, experienceId, experienceTitle, UserEmail, startTime, and endTime.

## Diagram

![dataModeling](./src/img/Comer%20data%20modeling.png)
