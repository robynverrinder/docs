# African Robotics Unit documentation

## Newcomers guide
If you're new to Git/GitHub, please see the [newcomers guide](newcomers-guide.md). It's a work in progress! Our git cheatsheet is [here](https://github.com/African-Robotics-Unit/docs/blob/main/git-cheatsheet.md).

If you find yourself explaining a particular workflow as part of a project that you think will be useful in other projects, rather document it on this repo and link to it. Let's lean on the side of "bad documentation which incrementally improves over time" instead of "no documentation and a dead repo"


## Projects appropriate for this organisation
There are many different types of code projects, and it doesn't make sense to manage all of them through the ARU structures. A rough guide to our policy is as follows:

Managed by the ARU:
- Libraries: code that has been refactored into a library that gets imported as part of other projects should be kept here. For example, a re-usable Python module which contains general-purpose code for optimisation.
- Projects relating to hardware: many projects (for example, a [robot leg](https://github.com/African-Robotics-Unit/foot-design-project) or [a sensor](https://github.com/African-Robotics-Unit/sensor-logger)) have software/firmware which relates to an evolving system. Multiple people might be working on this system, either collaborating at the same time or one after another. In cases like this, keep the code/docs/etc here, and keep it in sync with the physical system.

Managed individually
- Your own research experiments: aka "sandbox stuff", should be kept on your own branches and managed more or less as you want. It can serve as a nice private place to learn about Git/GitHub/etc without worrying about publicly messing up or having to deal with the admin of pull requests.
- Code for papers: again, rather host code and experiments for papers on your own GitHub account (or wherever else makes sense). These will invariably be abandonded after the paper is done, and you might want to showcase them on your own page for career purposes anyway.


## List of links
If you're looking for a resource for learning in a particular robotics related subject, check out the [list of learning links.](learning_links.md)

If you're looking for a contact within EBE or a company in Cape Town to help you procure something or provide a service, checkout the [list of contacts and companies.](contacts&companies.md)

## Please contribute
It's super simple to improve docs directly from GitHub:

- click on a file
- click the pencil icon on the right hand side to edit the file
- make your changes
- preview the file to make sure it renders nicely
- type a short message summarizing the change you made
- click "Create a new branch..." if you'd like someone else to check the change before it's made
- click "Commit changes"

You don't need to ask for permission

Otherwise, if something is wrong/unclear/whatever but you don't know how to change it, [raise an issue](https://github.com/African-Robotics-Unit/docs/issues/new)
