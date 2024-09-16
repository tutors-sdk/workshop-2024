## Cards & Resources

Cards & Learning Objects

[[toc]]

## Cards

The card metaphor is used throughout tutors as a simple visual feature to represent a variety of learning resources. In general the contents of a card are extracted from the following:

- A Markdown file, containing resource title + short summary
- An image, in png, jpg, gif or svg formats.

The title, image and summary will be presented on the card, along with a suitable icon.

The Learning resources are typically named to match the context, and are contained in a folder whose name is structured to encode the type of learning resource.

### Resource Names

Folder names convey the type of learning resource contained in the folder, with the first letters determining its type. Folders starting with the following names have a significance in Tutors:

|  Example                                                                                     | Source |
| -------------------------------------------------------------------------------------------- | ------------------------------------------  |
| [topic](https://tutors.dev/topic/reference-course/topic-01-typical)                   | [Top level course topic ](https://github.com/tutors-sdk/tutors-reference-course/tree/main/topic-01-typical)|
| [unit](https://tutors.dev/topic/reference-course/topic-01-typical)                    | [Group of learning objects within a topic  ](https://github.com/tutors-sdk/tutors-reference-course/tree/main/topic-01-typical/unit-1)|
| [side](https://tutors.dev/topic/reference-course/topic-02-side)                       | [Group of learning objects framed in a sidebar  ](https://github.com/tutors-sdk/tutors-reference-course/tree/main/topic-02-side/side-unit)|
| [archive](https://tutors.dev/wall/archive/reference-course)                           | [Downloadable zip file of resources  ](https://github.com/tutors-sdk/tutors-reference-course/tree/main/topic-07-reference/archive)|
| [book](https://tutors.dev/lab/reference-course/topic-01-typical/unit-1/book-a)        | [Step by step lab instructions, authored in markdown  ](https://github.com/tutors-sdk/tutors-reference-course/tree/main/topic-01-typical/unit-1/book-a)|
| [PDF book](https://tutors.dev/lab/reference-course/topic-02-side/side-unit/book-c)    | [Problem or exercises sheet, authored as a PDF  ](https://github.com/tutors-sdk/tutors-reference-course/tree/main/topic-02-side/side-unit/book-c)|
| [github](https://tutors.dev/wall/github/reference-course)                             | [Link to a GitHub repository  ](https://github.com/tutors-sdk/tutors-reference-course/tree/main/topic-01-typical)|
| [note](https://tutors.dev/note/reference-course/topic-01-typical/unit-2/note-1)       | [Single web page, authored in markdown ](https://github.com/tutors-sdk/tutors-reference-course/tree/main/topic-01-typical/unit-2/note-1)|
| [panelvideo](https://tutors.dev/topic/reference-course/topic-03-media)                | [A full screen width video, hosted in YouTube or HEANet](https://github.com/tutors-sdk/tutors-reference-course/tree/main/topic-03-media/panelvideo-1)|
| [paneltalk](https://tutors.dev/topic/reference-course/topic-05-panel-talk)            | [Full screen width  presentation in pdf format    ](https://github.com/tutors-sdk/tutors-reference-course/tree/main/topic-05-panel-talk/paneltalk)|
| [panelnote](https://tutors.dev/topic/reference-course/topic-04-panel-note)            | [Full screen width note](https://github.com/tutors-sdk/tutors-reference-course/tree/main/topic-04-panel-note/panelnote) |
| [talk](https://tutors.dev/talk/reference-course/topic-01-typical/unit-1/talk-1-intro) | [Standard presentation in pdf format  ](https://github.com/tutors-sdk/tutors-reference-course/tree/main/topic-01-typical/unit-1/talk-1-intro)|
| [web](https://tutors.dev/wall/web/reference-course.netlify.app)                       | [Link to an external web site  ](https://github.com/tutors-sdk/tutors-reference-course/tree/main/topic-01-typical/unit-2/web-1) |

 To sort the name alphabetically you may append numerals. To enhance meaning, append contextual keywords. For example:

| Folder Name            |
| ---------------------- |
| topic-01-introduction  |
| topic-02-learning-html |

For all file & folder names, avoid spaces within a file name.

| Do                     | Don't
| ---------------------- | -------------------- |
| topic-01-introduction  | Topic 01 Introduction
| topic-02-learning-html | Topic 02 Learning HTML

### File Names

Each resource will typically have the following files:

| File                   | Purpose                                                      |
| ---------------------- | ------------------------------------------------------------ |
| some-resource-name.md  | A markdown file, typically containing title + short summary for the resource |
| some-resource-name.png | Image to he used for the resource. Name must be the same as the .md file, image can be .png, .jpg. .jpeg, .gif |

The following filenames are reserved:

| File name       | Description                   |
| --------------- | ----------------------------- |
| course.md       | Title for course (mandatory)  |
| properties.yaml | Course properties (mandatory) |
| course.png      | Course image                  |
| calendar.yaml   | Course calendar               |
| enrolment.yaml  | Student enrolment file        |
| weburl          | link to external web site     |
| videoid         | id of  external video         |
| githubid        | link to github repo           |

