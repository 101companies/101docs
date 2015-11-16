# The 101Contribution Process

This process was designed to give as many  information as possible about the ongoing process to the contributor.
To keep things simple the contributor have to login to GitHub where he should have uploaded his own Code, that he wants to contribute with to 101wiki.

This guarantees consistent code, because the contributor don't have to change the code on more then one sources.
After login he choose one repository and optionally a folder inside, which he wants to commit. After giving the contribution a title, the system checks, if there is another contribution with the given name.
If there is another page with the selected name, the contributor will be redirect to the creation page to set a new title which is not given yet.

Optianally the contributor can give his contribution a description to picture his code. If none description is givenby the cotributor, there will be filled in some default description text. The description text will be placed in the main body of the wikipage corresponding to the contribution.

After creating the new page, the contributor will be redirected to his new created page and an email will be send to his mailaccount and also to the adminstrator mail account of 101wiki. If the contributor is not an administrator of 101wiki the page will be displayed as not verified to inform other users to be critical with the page's content. The verification of a page can be done by an admin after he has approved the page and the repository content. When this happens the contributor will be informed by mail.

The page can be edited by the author at any time. In theconfidence that any change by an editor is correct, the page will not bechanged to unverified again.
For visual representation of the contribution process, see the exetended diagram in our BPMN model.
 
## The contribution controller
The process in the controller is just as described. At first there are some checks for creating a page:
·        the user must be logged in
·        a repo link must be given (Repository and theFolder in the repo)
·        Name of contribution must not have a name thatwere already chosen and must not be nil.

Then the description is filled with a default text unless there was a given description text by the contributor. Next there is the decision if the page will be directlyverified if the author is an admin. After all these checks the page will be saved and an email will be send to the author. The contributor will be set to the role of a contributor by the system, which allows him to edit the wiki page of his contribution. 

## Thoughts about the Contribution Process
The contribution process was designed to be as easy to unterstand as possible. So the contributor should be guided through the process by detailed steps. Therefor we got the index page of the old contribution process and edited it to describe the process in a more detailed manner.

Additionally the question, when to send emails to the contributor and other system entities, had to be answered. We decided to inform the contributor any important action, because we wanted a transparent system for the users. We identified the following important actions, wich cause an email to be sent:
    - The creation of the contribution inside the 101wiki was successfull
    - The status of the wiki page changed to "verified"
If future works on the process conclude, that it is not enough to sent these two mails, it is possible to easily add more. But we decided to sent only two, because we don't want to overstrain the contributor.

Another question to be answered, was, if the verified status will be changed, whenever there is a modification by an editor. We concluded that just persons, who are responsible and validated will change the page only in a complaisant way.
