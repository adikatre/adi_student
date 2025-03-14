---
layout: post 
search_exclude: false
show_reading_time: false
permalink: /ppr
title: Personalized Project Reference
---

## Prompt #1
### Student-developed procedure with a parameter, sequencing, selection, and iteration
![snippet1](https://github.com/user-attachments/assets/eb6d065c-7546-445d-8262-1c7377fcec48)
Explanation: 
 - Procedure named `submitPoll` with a parameter (`formId`)
 - Contains sequencing because it goes through the process of getting question label, getting user input, transforming the string, fetching user data, and finally submitting the poll.
 - Uses selection for error handling using `if (!userResponse.ok)`
 - Uses iteration when transforming the string to cycle to the next poll form

## Prompt #2
### Call to above procedure
![snippet2](https://github.com/user-attachments/assets/06ed92d6-d6cc-42bb-a9e8-0dfbcbbb1b3c)
Explanation: 
 - Shows where `submitPoll` is called using `pollForm1`
 - Passes the relevant form ID to the procedure so it can handle user input for that specific poll form
 - Uses HTML `onsubmit` event that triggers the function and 

## Prompt #3
### Storing data in a list
![snippet3](https://github.com/user-attachments/assets/86e538be-203c-4474-ac5c-e44bfa0669ca)
Explanation: 
 - Shows storing data into `groupedPolls`, which is a list that maps each user name to an array of polls
 - Uses `data.forEach` to iterate through each poll item and pushes (aka. appends) to the -1 element of the array
 - Makes data management simple by storing related poll results under one key

## Prompt #4
### Using the same list
![snippet4](https://github.com/user-attachments/assets/ee894b05-bc15-46b7-a93a-590b0a4cfa37)
Explanation: 
 - Uses the data in `groupedPolls` to generate viewable table rows
 - Accesses each list using `Object.entries(groupedPolls).forEach(...)` and iterates through each item using a for loop (`polls.forEach(...)`)
 - Inserts `<input>` and `<button>` HTML elements based on the poll data of each user
 - Adds interactivity to page

<script src="https://utteranc.es/client.js"
        repo="adik1025/adi_student"
        issue-term="pathname"
        theme="boxy-light"
        crossorigin="anonymous"
        async>
</script>