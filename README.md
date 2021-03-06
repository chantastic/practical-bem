# Practical BEM
The TL;DR on BEM IRL

## Table of contents

1. [BEM anatomy](#bem-anatomy)
1. [Dialects](#dialects)
1. [Entity](#entity)
1. [Block](#block)
1. [Element](#element)
1. [Modifier](#modifier)

## BEM anatomy

```css
/*
 * Entity
 * A set of Elements and Modifiers related to a single Block
 */

/*
 * Block
 * The single root-class of your Entity
 */

.todo-item { }

/*
 * Element
 * Any number of Block-descendants
 */

.todo-item__mark { }
.todo-item__text { }

/*
* Modifier
* Any number of Block/Element modifiers
*/

.todo-item--complete { }
.todo-item--due { }
```

**[⬆ back to top](#table-of-contents)**

## Dialects
There are a few alternative BEM dialects. These examples use the most popular [`--` dialect](http://csswizardry.com/2013/01/mindbemding-getting-your-head-round-bem-syntax/) by Harry Roberts. Using `--` for modifiers removes a small ambiguity around [key/value modifiers](https://en.bem.info/methodology/naming-convention/#element-modifier).

**[⬆ back to top](#table-of-contents)**

## Entity
An Entity is a collections of Elements and Modifiers, related to a single Block. Entities are analogous to "modules" or "components" in other CSS philosophies.

**[⬆ back to top](#table-of-contents)**

Practically, Entities should live in a single file and be easily transportable to other projects.

## Block
A Block is the single, root-level, classes of any Entity.

```css
.todo-item { }
```

A bunch of Blocks:

```css
.report { }
.report-item { }
.report-subtotal { }

.person { }
.person-name { }
.person-child-list { }

.admin-person { }
.admin-person-responsibilities-list { }
.admin-person-responsibilities-item { }
```

### Relationships
* Block `belongs_to` Entity
* Block `has_many` Elements
* Block `has_many` Modifiers

**[⬆ back to top](#table-of-contents)**

## Element
An Element is anything nested in a Block.

*"Descendent" is actually a better word but `BDM` doesn't roll off the tongue.*

Here are some elements:

```css
.todo-item__mark { }
.todo-item__text { }
.todo-item__input { }
.todo-item__delete-button { }
```

Elements can have Modifiers. Though they're not preferred.

```css
.todo-item__delete-button--blingy { }
```

### Prefer Block-Modifiers
I find Block-Modifiers to be more maintainable than Element-Modifiers. In most cases, Element-Modifiers are partial Block-Modifiers waiting to grow up. You can avoid refactoring by preferring Block-Modifiers.

### No grandchildren

BEM supports only one level of element. If you find yourself reaching for Element-Elements (grandchildren), you have two options:
* Flatten out the Elements
* Create a new Block

#### This is incorrect
```
<li class="todo-item">
  <header className="todo-item__header">
    <span className="todo-item__header__text">Mow Lawn</span>
  </header>
</li>
```

#### Option 1: Flatten
```
<li class="todo-item">
  <header className="todo-item__header">
    <span className="todo-item__header-text">Mow Lawn</span>
  </header>
</li>
```

#### Option 2: New Block
```
<li class="todo-item">
  <header className="todo-item-header">
    <span className="todo-item-header__text">Mow Lawn</span>
  </header>
</li>
```

Now `.todo-item-header` can be composed with Elements from the `.todo-item` Entities.

```
<li class="todo-item">
  <header className="todo-item__header todo-item-header">
    <span className="todo-item-header__text">Mow Lawn</span>
  </header>
</li>
```

#### Why?

This may seem pedantic but it's critical to how BEM scales. Grandchildren introduce relationships into Entities. Once you have relationships, how many descendants do you cut off at? BEM makes it easy and cuts it off at 1.

### Relationships
* Entity `belongs_to` Block
* Entity `has_many` Modifiers

**[⬆ back to top](#table-of-contents)**

## Modifier
Modifiers are like HTML attributes. They alter Blocks and Elements. They can be **boolean** or **key/value** pairs.

```css
/* boolean */
.todo-item--hidden { }

/* named attributes*/
.todo-item--size_small { }
.todo-item--type_radio { }

/* Element_Modifier */
.todo-item__header--fancy { }
```

### Prefer Block-Modifiers
I find Block-Modifiers to be more maintainable than Element-Modifiers. In most cases, Element-Modifiers are partial Block-Modifiers waiting to grow up. You can avoid refactoring by preferring Block-Modifiers.

This code commonly gets outgrown:

```html
.todo-item__header--blingy { }
```

Prefer this by default:

```html
.todo-item--blingy { }
.todo-item--blingy .todo-item__header { }
```

### Alternatives
The BEM approach to Modifiers is hotly contested. This stems from a desire to type less or a misunderstanding of early BEM documentation. Granted, the early docs were in pretty poor English.

While I like variants with global states, e.g., `.is-visible`, it breaks the BEM model. More relevant, it makes utility-class libraries (e.g., minions.css and tachyons) harder to use.

### Prefer inline for generic visibility changes
For generic visibility changes, prefer an inline-style on the Block.

```html
<!-- ok -->
<div class="todo-item todo--visible"> ... </div>

<!-- preferred -->
<div class="todo-item" style="display: none"> ... </div>
```

### Relationships
* Modifier `belongs_to` Block
* Modifier `belongs_to` Element

**[⬆ back to top](#table-of-contents)**
