# Writing Pull Requests


Writing pull requests (PRs) is hard and every style of writing them is a whole world. Some styles are better than others (keep reading on this post for some examples), and eventually, the end result is getting your code merged.

Recently,on one of the weekly syncs with my team at GitHub, we talked about how to improve this process. As you would expect, everyone has a different opinion about the subject and a different vision. The more diverse a team is, the more variety you will have. While having a diverse team is definitely something you should be looking for, it also is the communication for interacting together, and in the case of writing PRs, as it is an essential part of our workflow, this has to be discussed.

On this post, my aim is to provide a framework on how to write PRs to be sucessfully accepted. This is, by no means, an ultimate tool to get your 15k LOC PR merged, but a set of guidelines I normally have in mind for every project I work on.

# Why use PRs
- Learning on the owner and reviewer side. Not only technical skills, also soft skills get formed with time.
- PRs can be used as additional documentation.

# Styles of working
During my professional career, I have seen different styles for merging work into a shared codebase:

## Wild West
You are supposed to write a PR to push your code, however it is not really enforced. Writing the PR takes as much time as writing the next feature, so you just push to master directly. It is the Wild West and you know you have to resolve a conflict because the VCS server rejected your push.
My personal take: Try to fix it and introduce obligatory PRs. In addition, block master (or your production branch) so code without a review cannot be pushed.


## The perfect workflow
Every commit represents only one thing. The reviewer will go normally over every commit you have done and review piece by piece.
This is a fictional example on how it would be adopting this approach:

Advantages:
It is clean, it is clear, it is easy to review
Disadvantages:
- If the review is not trivial, you likely have suggestions/feedback you want to incorporate. These changes will go to one of the previous commits you have in your history. This ends up on replaying commits and force pushes, affecting history each time. 
- You also take the risk of committing your work in a series of commits that the reviewer can disagree.
Unless your work is perfect from the beginning, you are likely rewriting your history several times during the process.



## Framework for 
I am sure you can 
