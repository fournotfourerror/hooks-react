# hooks-react

### useRef
```javascript
  import React,{useRef} from 'react'
export default function Login(){
    const inputElements=useRef({})
    const submitData=(e)=>{
        e.preventDefault()
        console.log(inputElements.current.name.value)
        console.log(inputElements.current.email.value)
        console.log(inputElements.current.mobile.value)

    }
    return(
        <>
            <form onSubmit={submitData}>
                <input type="text" ref={e=> inputElements.current['name']=e} placeholder="Name"/> <br />
                <input type="email" ref={e=> inputElements.current['email']=e} placeholder="Email"/> <br />
                <input type="mobile" ref={e=>inputElements.current['mobile']=e} placeholder="Mobile" /> <br />
                <input type="submit" value="Submit" />
            </form>
        </>
    )
}
```

## Memorization
*  Helpful for performance optimization
*  Works like a cache storage and stops re-rendering

### React.memo
```javascript
// Parent.jsx
import React,{useState} from 'react';
import Child from './Child';
import '../App.css'

const Parent=()=>{
    const [counter,setCounter]=useState(0)
    const updateCounter=()=>{
        setCounter(counter+1)
    }
    return(
        <div className="App">
            <h2> Parent Component : {counter} </h2>
            <button onClick={updateCounter}> Update counter value </button>
            <Child />
        </div>
    )
}

export default Parent;


// Child
import React,{memo} from 'react'
const Child=({number,changeNumber})=>{

    console.log("child")
    return(
        <>
            <h2> Child : {number} </h2>
        </>
    )
}

export default memo(Child);

```
