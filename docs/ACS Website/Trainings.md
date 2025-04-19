#Trainings

##Training Quizzes

The first step in gaining access to equipment is to complete the training quizzes associated with the equipment.

A score of 80.0% or higher is required to pass a training quiz. This is a hard-coded value, and cannot be changed.

Training quizzes by default will expire 1 year after completion, requiring the user to re-do them annually to maintain equipment access.

!!! note
    The terms *Training Quiz*, *Safety Quiz*, and *Training Module* are used interchangeably throughout this documentation.

###Editing Training Quizzes

Training quizzes can be created and edited in the "Manage Trainings" page.

By default, created trainings will be in the "Archived Modules" section. Trainings in the archive are hidden from users, and any equipment requirements associated with them will be paused. You can activate a quiz by hitting the green arrow icon next to it. You can also move an active training back to the archive by pressing the red button next to its name.

Training quizzes are made of block elements, that can be dragged into any order you want. They will be served to the user in the order specified as one continuous page. There are 5 building blocks available:

* **Question**: The main component of a quiz. Once a question is created, it can be made either *Multiple Choice* (where one answer is correct), or *Checkboxes* (where multiple answers are correct, and all correct one must be selected to score). 
* **Text**: These are best used for conveying training content. Text blocks fully support Markdown editing for fine-control over how the content is displayed.
* **Video**: This block can be used to embed a YouTube video. The video must be public and not age-restricted for this to work. 
* **Image**: This block allows you to include a static image. The image must be hosted by some other CDN, and then you paste in a link to that image.
* **PDF**: This block displays a PDF in an embedded PDF reader, ideal for manuals and other long-form written content. Just like with images, the PDF needs to be hosted elsewhere and linked into the training.

###API Triggers

Completing a training quiz can optionally trigger an API call. As an example at RIT, completing the "3D Printing Self-Serve Training" automatically adds the user to the proper workgroup on 3DPrinterOS, automating access to the 3D printers.

###Training Holds

If users fail the same training too many times in a day (default 3), they will receive a training lock. Training locks stop them from attempting that specific training again until the next day, unless a [Maker Mentor](./User%20Management.md#mentor-role) or [Staff](./User%20Management.md#staff-role) removes it from their account first. Account holds are a way to ensure users cannot brute-force their way through the answers.

###Assign Trainings to Equipment

Non-archived trainings can be added as a requirement on the "Manage Equipment" page. See [Equipment Management](Equipment Management.md) for more information.

Trainings can be removed or added to equipment at any time. Adding a new training will stop users from using the equipment until the new training is complete. They will not have to re-complete the [Staff Approval](#staff-approval). Editing a training does not require users to re-take the new version.

##Staff Approval

Once all the required trainings have been completed, users have the ability to complete the Staff Approval with either a [Maker Mentor](./User%20Management.md#mentor-role) or [Staff](./User%20Management.md#staff-role). 

Users will have to demonstrate competency to the mentor or staff before getting a sign off to use the equipment. There is no structured or standard format to this, and is at the discretion of the implementing shop. 

Once the approval is complete, the mentor or staff can check off the approval in the user's account on the "People" page. The user will now be able to use equipment.

Mentors and staff are able to self-approve for equipment, but a flag warning of potential abuse is generated for further review.

Staff have the ability to manually create and approve checks for users before they finish all training modules. This'd result in them getting immediate equipment authorization as soon as the training modules are finished. 