# Trello-Board using Nuxt 3

- Mock a trello board by using nuxt3.
- Read the [Nuxt 3 documentation](https://nuxt.com/docs/getting-started/introduction) to learn more.
- The tutorial is from [Vue-school](https://vueschool.io/lessons/bootstrap-the-project-trello-board).

## Dependency packages

1. nuxt3
    ```bash
    npx nuxi init <project-name>
    ```
2. tailwindcss
    ```bash
    npm install --save-dev @nuxtjs/tailwindcss
    ```
3. vue-draggable
    ```bash
    npm install vuedraggable@next
    ```
4. nanoid
    ```bash
    npm install nanoid
    ```
5. Vue-use
   ```bash
   npm install @vueuse/nuxt
   ```
6. You can install all of them above by commanding:
    ```bash
    npm i vuedraggable@next nanoid @vueuse/nuxt
    ```

## Key points

1. [useKeyModifier](https://vueuse.org/core/useKeyModifier/#usekeymodifier)
    - If you want to track whether a keyboard is pressed or not, you can utilize this handler.
2. [onKeyStroke](https://vueuse.org/core/onKeyStroke/)
    - If you want to listening on if a key is keydown, pressed, holded, you can use it.
3. [VueDraggable](https://github.com/SortableJS/Vue.Draggable)
    - Based on [Sortable.js](https://github.com/SortableJS/Sortable).
    - When use this component, you don't need to use `v-for`, and you can drag items in an array.
    - `v-model`: which data you want to be draggable.
    - `item-key`: which attribute to be used as `:key`.
    - `group`: 
       1. A string to name your groups which is able to seperate different kinds of array that you don't want to mix up. For example you can name `parent` and `child` for each kind of array.
       2. An object contains `name` and `pull`.  The `pull` value can be `true` or `'clone'`.  While setting to `true`, dragging item would move it; and `clone` means to copy item to another array.
       3. See more detail [here](https://github.com/SortableJS/Sortable#options).
    - `handle`: to set which element can be draggable (default setting is all parts of item can be dragged.)
4. Use `defineProp()` with typescript
    - In old javascript, we use `defineProp({ todo: Object })` for accepting the prop `todo`.
    - In typescript, we have a custom type called `Todo`, but we can't just utilize as primitive type.  So we need to write `defineProp<{ todo: Todo }>()`.
5. Use `useLocalStorage` from VueUse
    - Rather then using native API, using useLocalStorage is an easier way.
    - Use `useLocalStorage` to replace `ref` if you want to interact with an reactive state which stores in localStorage (like theme, preference setting, input data).
6. In typescript, `defineEmits` syntax looks like this:
    ```javascript
    /*  
        e: event function name 
        payload: data type of variable that you want to pass back to parent component 
    */

    const emit = defineEmits<{
        (e: "delete", payload: ID): void;
    }>();
    ```
7. Use `nanoid()` to add UUID
8. Create fake input or textarea
   - There are many components in trello which look like text or title.  When user click on these items, they become editable `<input>`.
   - Here is the step to create it:
        ```javascript
        // First, declare a data to control its state
        const focused = ref(false)

        <input @focus="focused = true" @blur="focused = false" />
        ```
        ```javascript
        const focused = ref(false)

        // Then add style for focusing state to decorated like input field, and blur state to look like title
        <input 
            @focus="focused = true" 
            @blur="focused = false"
            class="bg-transparent focus:bg-white focus:outline-1"    
        />
        ```
9.  `nextTick()` to control the next action after create/update/delete something.
   - If you want to focus the element after it is created:
        ```javascript
        function create() {
            const item = {}

            // add item
            parent.value.push(item)

            nextTick(() => {
                (document.querySelector(".parent:last-of-type .item") as HTMLInputElement).focus()
            })
        }
        ```
10. `watch()` to update data to server after each time data change
    ```javascript
    /* if you want to communicate with backend server... */
    
    watch(
        columns,
        () => {
            // ajax request
        },
        { deep: true }
    );
    ```
11. Listening on `@keydown.backspace` to delete item
    ```javascript
    <input @keydown.backspace="text === '' ? deleteItem(item.id) : null" />
    ```

## Setup

```bash
# yarn
yarn install

# npm
npm install

# pnpm
pnpm install
```

## Development Server

Start the development server on http://localhost:3000

```bash
npm run dev
```

## Production

Build the application for production:

```bash
npm run build
```

Locally preview production build:

```bash
npm run preview
```

Check out the [deployment documentation](https://nuxt.com/docs/getting-started/deployment) for more information.

## Resource

- [GitHub repo](https://github.com/SortableJS/vue.draggable.next)