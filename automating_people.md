# Automating People

During my previous job, I collaborated with two colleagues from different teams to develop course material for a comprehensive training program. The curriculum covered various subjects such as Agile/Scrum, Foundational Linux, Cisco Routing and Switching, Network Automation with Python and Ansible, and Kubernetes. While contributing to all topics, my primary focus was on creating content for Kubernetes and designing the final exercise. Our goal was to empower students with limited experience to gain a solid foundation in these technologies and become productive. I meticulously crafted 146 labs, encompassing areas like Bash scripting, Docker, Kubernetes, and RHEL 8. An Agile approach was incorporated to encourage student collaboration throughout the day. The labs primarily consisted of tutorial-style guides, while key conceptual explanations were provided separately. Students were encouraged to develop their own reference materials for the written and practical exams. The exams were designed to challenge students to apply their self-authored steps, mirroring the process of learning from real-world documentation. With a relatively short three years of experience in Kubernetes and application development, I had to find a way to fit almost everything I learned into a about a month and a half of class time (not including weekends). To refine the labs, I conducted extensive testing with early arrivals at the office, identifying common issues such as overlooking important details and struggling with implied steps. Based on feedback, I iteratively improved the labs until they were accessible and required minimal modifications. This process taught me valuable lessons in document structure, formatting, cognitive empathy, and feedback.  That's not to say I couldn't come up with more if you gave me long enough to think on it.

## Cognitive Empathy
This is probably the most important thing I needed to learn when writing documentation or labs. Not everybody reading the documentation is at the same level, and they most likely aren't at your level of proficiency or else they probably wouldn't be looking up a guide on how to do it. Focus on:
1. Simplicity
2. Why and How
3. Provide context
4. Anticipate Questions
    - You need to put yourself in your least knowledgable reader's shoes and try to ask yourself "does this make sense if I'm brand new to this".
5. Visual Aids
6. **TEST**

### Code Simplicity
Code blocks are awesome tools for giving someone a clear cut method of doing something, but you need to be careful of the complexity of the code you put into a code block.  Some of you "heavy hitter" engineers out there will probably disagree with me on this, because less code is always better in production, but for documentation **simplicity and readability are almost always better**. (Unless you're writing documentation on specifically how to write awesome code, obviously)

Here is an example of something I saw recently in the Talos Docs for [airgapped installs](https://www.talos.dev/v1.4/advanced/air-gapped/):

```
for image in `talosctl images`; do \
    docker tag $image `echo $image | sed -E 's#^[^/]+/#127.0.0.1:6000/#'`; \
done
```
- Unless this is an advanced tutorial in bash or automation, I wouldn't use this in a how-to guide. If something doesn't work in this command, someone less experienced will probably be have a hard time troubleshooting this.

Instead I would write it like this:


> For each images print `talosctl images` you have already pulled, tag them with the local registry we configured.
Example is for `ghcr.io/siderolabs/flannel:v0.21.4`
```bash
docker tag ghcr.io/siderolabs/flannel:v0.21.4 127.0.0.1:6000/siderolabs/flannel:v0.21.4
```

It doesn't take any more time to write into the docs and the now an **inexperienced** user knows what they're doing. Let the users automate it when they get proficient.

> Note this is especially try for **beginner** or **quickstart** guides where you most likely have the least experienced users.

### Why AND How

There should never be anything in a guide which just shows people how to do something.  If there is no explanation of why, a user who doesn't already know they why is either going to **give up** or spend **more time troubleshooting** than they need to. A simple type in a code block could turn into hours of troubleshooting for someone who doesn't understand why they're running things and then your software is going to forever feel complicated to them.

### Provide Context

Context helps give a better understanding of the big picture.  This will give users a reason to want to read the docs as well as give them a better understanding if or when things go wrong. I usually Give a short overview of each subject immediately **after each Header**.

### Anticipate Questions

This is where you really need to put yourself back in a beginner's shoes. If you can make yourself remember the things which we're difficult, you can almost always think of most of the questions users are going to ask.
> **NOTE:** Be careful not fill up too much space with paragraphs of words trying to answer every possible question.  Instead use something like a Troubleshooting or FAQ section to combine those types of things.

### Visual Aids

Visual aids are extremely helpful. Moving files around is a good example of something that can seem complicated when written strictly in text.

Example: move `file1` from your working directory into `folder3` and then move `file2` from `folder1` into your working directory.
- The structure should now look like this:
```bash
.
├── file2
├── folder1
├── folder2
└── folder3
    └── file1
```

This just helps users get verification that they are doing things right.

> A quick diagram can help too sometimes. If you don't have visio, you can make some professional looking diagrams in `draw.io`

### TEST!

You wouldn't put code into production which hasn't been fully tested in a way it's run in the real world. Don't do it for your documenation either. Always give the documentation to someone who **knows less** about the subject than you.  If you just have someone at the same level as you on the subject read through it, they will miss all the important problems your users are going to run into or pieces which aren't clear.

## Formatting and Structure

At first I thought people just need to learn how to pay more attention to the details in the guides.  Most of the issues were not because the labs were missing information or inaccurate, but when **over 50%** of my test students were having problems with the **exact same** things, I realized there had to be something I could change to reduce the number of issues.

### Structure

When I say structure, I'm talking about flow of the content.  Consistency is important.  If all the documentation has a similar flow and appearance, people will learn to follow it much better as they switch to another lab or walk-through.

I generally kept to the following structure overall:
- **Introduction**
    - Some sort of general description to explain what is contained in the documentation.
    - This primes people with the big picture for what they are about to learn.
    - Use links to the Headings.
- **Overview**
    - Used for long labs where people would half to scroll more than a few pages up or down to find sections.
- **Body**
    - Consistent use of Markdown Heading levels.
    - This is the bulk of the material.
    - Nearly every sentance should contain a **HOW** and **WHY**. I'll talk more about that in Cognative Empathy.
- **Summary and Closure**
    - While this is subjectively more important when giving verbal instructions, it can still be helpful for reinforcing the content if the guide is meant to teach complex information.

### Headings

The word choices for headings are extremely important in keeping people tracking along with is being written.  A lot of people will skim content and not read, so if the headings paint a general picture, they can still get a general idea if they didn't read *everything*.

### Lists

Don't be afraid of using lists, to draw out multiple main subjects of a description.

Instead of:
```markdown
To configure the Ceph storage cluster, at least one of these local storage options are required: Raw devices (no partitions or formatted filesystems), Raw partitions (no formatted filesystem), LVM Logical Volumes (no formatted filesystem), Persistent Volumes available from a storage class in block mode
```

Use:

```markdown
To configure the Ceph storage cluster, at least one of these local storage options are required:
- Raw devices (no partitions or formatted filesystems)
- Raw partitions (no formatted filesystem)
- LVM Logical Volumes (no formatted filesystem)
- Persistent Volumes available from a storage class in block mode
```

- The good example was taken from [rook-ceph](https://rook.io/docs/rook/v1.11/Getting-Started/quickstart/#prerequisites)

### **Bold Words** and `Inline Code`

All three of these can be used to get a point across extremely efficiently if used correctly.

I mostly used these the following ways:
- **Bold Words**
    - **Important points** or **sections** of a statement **which might be missed** in a long line of text if someone is skimming or reading quickly, but **are extremely important**. 
    - I will also sometimes use them for **list items** which are in a long list and **could get lost** in the large chunk of text.
- Inline Code
    - Inline code is great when there really isn't a need for a full code block, but you are describing something you want to make easy to copy like a `filename` or `cli command`

### Verbosity

On a page with a **TON** of text between headers, lists or code blocks, most people will end up skimming and not reading most of what you have to say. Try to limit what you have to say to a few sentances.  If it's hard to shorten it:
- Slim it down into a list of main points
- Use a link to point to another document with more information on the particular subject
> Add a note to catch peoples eye and break up the wall of text

## Feedback

When I was teaching the course, sitting side-by-side you'd think it would be easy to get feedback on what people were having problems with. It was not. Even with direct interaction, a lot of people will want to try to solve the problem themselves first, or are afraid of sounding dumb by reaching out and asking a question. It's important to take good notes when people do bring up problems. Ask questions about how they got to the state they're in and if they saw anything wrong with the documentation.  Personally, I like to have separate troubleshooting sections in each document. Take that feedback and use it to improve the documentation.  Maybe there's a step a lot of people are getting stuck on or having issues with and the information is there, you just need to highlight something that's already there.

## Closure

This isn't all inclusive by any means, but it covers most of what I learned about why my own documentation was failing to get the point across.  Again, I will say, the most important part of this for me was learning how to think like I was a beginner again, but by combining all of these points into my documentation, people starting gaining a much better understanding of what I was writing about.  I like to say writing documentation is like automating people.  It turns out people aren't exactly like computers which do the exact some thing every time with the same line of code.
