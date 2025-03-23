---
title: "Why Typescript?"
meta_title: "Why Typescript?"
description: "Usages of typescript."
date: 2023-05-26T05:00:00Z
image: "/images/mapbox.jpg"
categories: ["Application", "Data"]
author: "Viraj"
tags: ["mapbox", "javascript", "mapping", "tutorial"]
draft: false
---

Checking typos in the compile time is better than run time. We have lots of experience of having errors in our javascript apps because of typos. So by using typescript these typo errors have been thrown away.

While in coding with IDEs like VS-CODE, The errors can be seen therewithal. Since the compiler is acknowledged about the type of the variable, developers can see the functions through autocomplete features of the IDE.

When it goes to a large and complex codebases, the ability of maintaining the code is higher when using typescript.

const number = (n: number) => n + 2;
The above code is written using arrow function. So the n variable is a number and it has been defined as a number by Typescript. In addition this function returns n+2. So it returns a number too. So if we rewrite like this,

const number = (n: number): number => n + 2;
this says the function returns a number. But it is not necessary to include the return type here since typescript knows it well. So the first code snippet is the better.

Now let’s walk through a react example. Review the following code.

```typescript
const NameBadge = ({ name }: { name: string }) => {
  return (
    <section className="badge">
      <header className="badge-header">
        <h1 className="text-5xl">HELLO</h1>
        <p>My name is…</p>
      </header>
      <div className="badge-body">
        <p className="badge-name">{name}</p>
      </div>
      <footer className="badge-footer" />
    </section>
  );
};
```

This NameBadge is a react component. So if a developer uses this name badge, he/she should pass the name as a property ‘name’ to this component as follows. So the ‘name’ is a variable which type is String.

```typescript
<main className="application">
    <NameBadge name="Viraj" />
  </main>
```

So what happens if we write call this NameBadge component without name property. Yes, it leaves us an error.


Cooool !!!! This the practical usage of the typescript.