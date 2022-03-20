# Section 13: Advanced DOM Manipulation

## How the DOM really works

In the DOM there are different types of notes. Every single node in the DOM tree of the type Node. All elements are of type Element which in turn has a type called HTMLElement which in turn have types like HTMLButtonElement, HTMLDivElement etc.

> Because of inheritance, any HTML element can access node methods like .closeNode() or element methods like .closest() or .querySelector()

## Smooth scrolling

```JSX
btnScrollTo.addEventListener('click', function (e) {
  const s1coords = section1.getBoundingClientRect();
  console.log(s1coords);

  console.log(e.target.getBoundingClientRect());

  console.log('Current scroll (X/Y)', window.pageXOffset, window.pageYOffset);

  console.log(
    'height/width viewport',
    document.documentElement.clientHeight,
    document.documentElement.clientWidth
  );

  // Scrolling: old/backcompatible way
  // window.scrollTo(
  //   s1coords.left + window.pageXOffset,
  //   s1coords.top + window.pageYOffset
  // );

  // window.scrollTo({
  //   left: s1coords.left + window.pageXOffset,
  //   top: s1coords.top + window.pageYOffset,
  //   behavior: 'smooth',
  // });

  // Scrolling: very modern way
  section1.scrollIntoView({ behavior: 'smooth' });
});
```

## Event propagation

## DOM traversing

## Building a tabbed component

```JSX
tabsContainer.addEventListener('click', function (e) {
  const clicked = e.target.closest('.operations__tab');

  // Guard clause
  if (!clicked) return;

  // Remove active classes
  tabs.forEach(t => t.classList.remove('operations__tab--active'));
  tabsContent.forEach(c => c.classList.remove('operations__content--active'));

  // Activate tab
  clicked.classList.add('operations__tab--active');

  // Activate content area
  document
    .querySelector(`.operations__content--${clicked.dataset.tab}`)
    .classList.add('operations__content--active');
});
```

## Reveal elements on scroll

## Lazy loading images

## Building a slider component

## Lifecycle DOM Events

## Script loading: defer and async
