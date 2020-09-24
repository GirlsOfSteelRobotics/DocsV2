.. _code-reviews:

Code Review
===========

|github-code-review|
*Note:* 
In the video, I could not request changes because it was my own review. When you are reviewing someone else's code and find something they should fix, click that instead of "Comment"

Also note that the mac build hadn't finished, but you should wait for all of the builds to be green before running the merge.

Pull requests and code reviews are a chance for the team to look over your changes and make sure they are good and ready to be merged into master. Github will
also attempt to build your code and make sure it can be merged into master, so we can mitigate mistakes and see exactly what the changes you are proposing are
before before we give it the OK.

It is everyone's responsibility to review other peoples code, so we can increase the code quality, keep informed about all the new features going into the robot,
and make sure nothing gross gets in.


What To Look for
----------------

When you are reviewing someone else's code, the first thing to thing you should do is look through it and make sure it makes sense to you.

- Are the class / variable names good? Will you be able to remember what :code:`public IntakeCommand(boolean move)` / :code:`new IntakeCommand(true)` means in a month when you are creating autonomous modes?
- If there is complicated logic, is it commented well enough so you someone else can come back to it later and quickly pick up what is going on?
- Is this change even necessary? Would it be better to modify an existing class to add more functionality instead? It is easy to get out of sync and duplicate logic which can be super confusing later.
- Are there any spelling errors or style guide errors?
- Note, not all comments have to be negative. You can always give props during reviews if someone did something really cool or in a new and innovative way.

Requesting changes
------------------

When you look at a review on GitHub, you can add comments, and if you think something should be changed, you block the pull request until those comments are addressed.
In the "Files Changed" tab of the PR, you can click on a line and leave a comment, which will start your review. When you are done, you can click the green button, and optionally
request that they fix the problems, or you can "Approve" the review.

Merging Changes
---------------

Once the review has been finished, all the CI builds pass, and one of the software mentors or student lead have given approval, the PR can be merged. We use the "Squash and Merge"
approach, which takes all of you commits (if you have multiple), and combine it into one commit before merging it into master. This allows us to have a nice and neat linear history,
which makes it easier to rollback changes if a bug pops up.


.. |github-code-review| image:: images/github-code-review.gif