# Entry 1
##### 11/04/24

## **Context**:
In this project, I will be makeing a civilization or city-building simulator game with my Zixuan Yu and William Zheng. The tools that we've testing with to see the best fit to use for the project are [Unity](https://unity.com/) and [Godot](https://godotengine.org/). These tools support [C#](https://www.w3schools.com/cs/index.php), a programming language I plan to use for the project.             

## **Content**:

#### **Tinker #1**:

From what I learned from Samuel, I started by using `npx create-react-app appName`(This is to download a React app in my ide). Next, I created a components folder where I made a `Navbar`(I kept in mind to name my components using capitalize first letters). I replaced `<Navbar />` with everything inside the `<div>`. so that it shows what I wrote for the navbar.

```js
function App() {
  return (
    <div>
      <Heading />
    </div>
  );
}
```

*This is in the `Navbar`:*

```js
function Heading() {
  return (
    <div>
      <h1>Hello World</h1>
    </div>
  );
}
```

#### **Tinker #2**:

After making the `Heading`, I thought it will be a cool idea that since I can reuse a function, I can use the repeat the Heading. Thus, I made a second component `Para` to experiment with. Below, I labeled in `comments` the amount of times the `Heading` and `Para` will repeat.

```js
function App() {
  return (
    <div>
      {/* first */}
      <Heading />
      <Para />
      {/* second */}
      <Heading />
      <Para />
      {/* third */}
      <Heading />
      <Para />
    </div>
  );
}
```

*The function for `Para` is:*

```js
function Para() {
    return (
      <div>
        <p>Hello World Hello World Hello World Hello World Hello World Hello World Hello World Hello World Hello World Hello World Hello World Hello World Hello World Hello World Hello World Hello World Hello World Hello World Hello World Hello World Hello World Hello World Hello World Hello World</p>
      </div>
    );
  }
```

With this, I am starting to get the hang of React which makes creating the front end of a website much more easily by separating each component.

## *Sources**:

* [Freecodecamp](https://www.freecodecamp.org/learn/front-end-development-libraries/#bootstrap)
* [React document](https://react.dev/learn)

## **EDP**:

For this entry of the project, I am currently on the first step of the engineering design process. This is where I define the problem my project will strive to solve. The problem that my projects will solve is students being ignorant of their gades. Being ignorants of your grades can pose a big problem to a person's school career because they will not know where they need to improve and whether to tell a teacher about a mistake in their grade. Thus, my project will give the students a grade book where they will see their assignments real time. Unlike other grade books, RiceGrades will provide a more fun and interactive grading system for teachers, students, and parents.

## **Skills**:

As I complete the first step of the project, I tried to incorporate some skills that I can learn and retain. For example, I broke down my tinkering into smaller pieces so that each time I'm free, I will learn a new concept of React which helped me practice **problem decomposition**. I also articulated ideas with my partners as we meet in the library every week to delegate our roles(We decided that Samuel will be the middle man since he has knowledge on React and Firebase while I will learn React and Yu learns Firebase) and plan on what we will work on. During these meetings, we would additionally circulate ideas about how we want the website to look like. For example, I came up with the idea of the color themes and Samuel wanted to include a quote that we all agree on. This is where I practiced my skills **time-management** and **communication**.

[Next](entry02.md)

[Home](../README.md)
