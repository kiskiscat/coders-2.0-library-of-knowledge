# Модальные окна

```html
<dialog id="dialog">
    <form>
    <button type="submit" aria-label="close" formmethod="dialog" formnovalidate>X</button>
    <h2 id="dialogid">MLW Registration</h2>
    <p>All fields are required</p>
    <p>
        <label>Name:
           <input type="text" name="name" required />
       </label>
    </p>
    <p>
       <label>Warranty:
          <input type="number" min="0" max="10" step=”1” name="warranty" required />
        </label>
    </p>
    <p>
       <label>Power source:
           <select name="powersoure">
          <option>AC/DC</option>
          <option>Battery</option>
          <option>Solar</option>
           </select>
       </label>
    </p>
    <p>
       <button type="submit" formmethod="post">Submit</button>
    </p>
 <hr/>
    <p>Additional buttons</p>
      <button id="jsbutton">JS close</button>
      <button id="reset" type="reset">Reset</button>
    </p>
  </form>
</dialog>
```
