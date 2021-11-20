# mini-component
A minimal component that creates an Element with Attributes, Child and Text nodes

A very small but full-featured component function written in 14 lines of
JavaScript.

### Source Code:
```javascript
export const element = (tag, attributes, ...children) => {
  const el = document.createElement(tag)

  for (let key in attributes) {
    el.setAttribute(key, attributes[key])
  }
  children.forEach(child => {
    if (typeof child === 'string') {
      el.appendChild(document.createTextNode(child))
    } else {
      el.appendChild(child)
    }
  })
  return el
}
```
Yup, that's the whole thing. No dependencies. Just a simple exported module.
# Usage
### First arg:
Any quoted HTML tag, such as `'h1'` or `'div'` as long as
`document.createElement` understands it.

### Second arg:
A JavaScript object with `{key: string, key: string}` structure that
gets translated to attributes. There is no limit to object
members. Any key and value that `setAttribute` understands is allowed.

### Third arg:
Can be either a text string, which creates a DOM Text Node, or a child
element. The child can be any component, function or element that is
legal HTML, DOM JavaScript, or preferably, another `mini-component`
component. Child nodes can nest without limit.

```javascript
let foo = element('div',{class: 'myclass'},`this is a text node`)
let bar = element('div',{class: 'barclass'} childComponent)
```
# Example
```javascript
import {element as el} from 'mini-component'
import Dropdown from './Dropdown'      // Example of imported component function

const burger = el('li',{class:'burger dropdown'},`☰ `) // Text node element
const Nav = () => {                    // Function to be invoked from higher level
  let nav = el('nav',{class: 'topNav'} // `nav` is the returned component
               el('ul',{id:'toplist'}
                  el('span',{class:'menu'}
                     burger               // Nested text element
                     el('li',{},          // Empty Attributes, but required
                        Dropdown(         // Child component from import
                          {               // Props passed to component
                            label:'times'
                            submenus: {
                              sunday:{width:'50%',height:'100%'}
                              weekday:{width:'50%'}
                            }
                          }
                        )
                     )
                  )
               )
  )
  return nav
}

```
# Comments
I got the idea for this from non-working code on a forgotten
site and wrote my own version. If anyone knows who that author is, I'd
appreciate knowing so I can give proper attribution for his contribution.

I started to write a JSX translator for this, but realized that it's
easy to code and read all by itself, so I abandoned it. It was kind of
a fun Babel project though, while it lasted, so worth the
effort. Someday, maybe.
