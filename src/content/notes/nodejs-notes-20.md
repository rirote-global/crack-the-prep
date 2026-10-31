---
title: "Mail sending Nodejs Script"
part: "Node.js Notes"
track: "nodejs"
kind: "notes"
updated: "2026-09-02"
source: "Nodejs.docx"
draft: false
order: 20
description: "Node.js — Mail sending Nodejs Script."
---
```js
const fs = require('fs');

const xlsx = require('xlsx');

const nodemailer = require('nodemailer');

const { exit } = require('process');
```
// download the above packages

// Load your Excel file

const workbook = xlsx.readFile('./list.xlsx'); // Path for the sheet in your local folder

const sheetName = 'Bytedance'; // Change to the name of your sheet

```js
const worksheet = workbook.Sheets[sheetName];

const data = xlsx.utils.sheet_to_json(worksheet);
```
// Email configuration

```js
const newTransporter = ()=>{

return nodemailer.createTransport({

pool: true,

host: "smtp.gmail.com",

port: 465,

secure: true,

auth: {

user: "you@example.com",

pass: process.env.SMTP_PASSWORD
```
},

```js
});

}

const transporter = newTransporter();

const sendEmail = async (row) => {

const { Name, Company, Email, Role, Link } = row; // Adjust column names accordingly

const nameParts = Name.split(' ');

const name = nameParts[0];

const mailOptions = {

from: 'Your App <you@example.com>',

to: Email,

subject: `Request for an Interview Opportunity - ${Role} at ${Company}`,

html: `
```
<p>Greetings ${name},</p>

<p>I'm Your Name, a Software Developer at Your Company. I got to know through your linkedin post that <b>${Company}</b> is looking for a <b>${Role}</b>, therefore, I have mailed you to tell you about myself. I have:

<ul>

<li><b>X years</b> of hands-on experience in <b>your domain</b></li>

<li><b>X years</b> as a <b>Your Role</b> at <b>Your Company</b></li>

<li>Worked extensively in <b>JavaScript, Typescript, React & Next.js</b></li>

<li>Familiar with <b>REST, GraphQL, monorepo codebases, Jest & React Testing library</b></li>

<li>Worked on a product with <b>a notable scale or result</b>.</li>





</ul>

<p>Currently, I am <b>serving a notice period</b> and can <b>join within 15 days</b> of receiving an offer. A little help from your side can significantly help my career.</p>

<p>PS: I have attached my <b><a href="https://example.com/resume">Resume</a></b> & <b><a href="https://www.linkedin.com/in/your-profile/">Linkedin Profile</a></b> ${Link !== undefined ? \`& <b><a href=${Link}>${Role}</a> </b> Opening\`: '' } for you to take a look at. If you find me suitable, please help me with an Interview Opportunity at ${Company}.</p>

<p>

<p>

Thanking You<br>

Regards,<br>

<b>Your App</b> <br>

<b>Contact No: +91 9876543210</b><br>

</p>\`

```js
};

try {

await transporter.sendMail(mailOptions);

console.log('Email sent to', Email);
```
} catch (error) {

```js
console.error('Error sending email:', Email, error);

}

};

const sendEmailsSynchronously = async () => {

for (const row of data) {

await sendEmail(row);
```
await new Promise((resolve) => setTimeout(resolve, Math.random()\*90000)); // Pause for 1 minute (adjust the duration as needed)

```js
}

console.log("Done Sending mails")
```
exit()

```js
};
```
// Call the function to send emails

```js
sendEmailsSynchronously();
```
