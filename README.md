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

// useRef with onChange event handler
import { useRef } from "react";

const Hanu=()=>{
    const formData=useRef<any>({name:"Naveen", email:"naveen@gmail.com"})

    const submitForm=()=>{
        console.log(formData.current.name)
        console.log(formData.current)

    }

    console.log(formData.current.name)

    return(
        <form>
            <input name="name" placeholder="Name" defaultValue={formData.current.name} onChange={(e:any)=> formData.current.name=e.target.value}/>
            <input name="email" placeholder="E-mail" ref={(e:any)=>formData.current.email=e?.value} />
            <button type="button" onClick={submitForm}> Register </button>
        </form>
    )
}

export default Hanu;
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

### useCallback
```javascript
// Parent.jsx
import React,{useCallback, useState} from 'react';
import Child from './Child';
import '../App.css'

const Parent=()=>{
    const [counter,setCounter]=useState(0)
    const [inputValue,setInputValue]=useState("")
    const updateCounter=useCallback(()=>{
        setCounter(counter+1)
    },[counter])

    const updateInputValue=(e)=>{
        setInputValue(e.target.value)
    }
    return(
        <div className="App">
            <h2> Parent Component : {counter} </h2>
            <input type="text" value={inputValue} onChange={updateInputValue}/>
            <button onClick={updateCounter}> Update counter value </button>
            <Child counter={counter} changeNumber={updateCounter}/>
        </div>
    )
}

export default Parent;

//Child.jsx
import React,{memo} from 'react'
const Child=({counter,changeNumber})=>{

    console.log("child")
    return(
        <>
            <h2> Child : {counter} </h2>
            <button onClick={changeNumber}> Update Child Counter </button>
        </>
    )
}

export default memo(Child);
```

### useMemo
```javascript
// With out useMemo -> Takes time when clicking on button
import React,{useCallback, useState} from 'react';
import Child from './Child';
import '../App.css'

const Parent=()=>{
    const [counter,setCounter]=useState(0)
    const [inputValue,setInputValue]=useState("")

   

    const updateCounter=useCallback(()=>{
        setCounter(counter+1)
    },[counter])

    const updateInputValue=(e)=>{
        setInputValue(e.target.value)
    }

    let output=0;
    for(let i=0; i < 500000000; i++){
        output++
    }


    return(
        <div className="App">
            <h2> Parent Component : {`${output} - ${counter}`} </h2>
            <input type="text" value={inputValue} onChange={updateInputValue}/>
            <button onClick={updateCounter}> Update counter value </button>
            <Child counter={counter} changeNumber={updateCounter}/>
        </div>
    )
}

export default Parent;

// With useMemo takes less time from 2nd time updation of state (Memorizes the value)
import React,{useCallback, useState, useMemo} from 'react';
import Child from './Child';
import '../App.css'

const Parent=()=>{
    const [counter,setCounter]=useState(0)
    const [inputValue,setInputValue]=useState("")

   

    const updateCounter=useCallback(()=>{
        setCounter(counter+1)
    },[counter])

    const updateInputValue=(e)=>{
        setInputValue(e.target.value)
    }

    const childSampleOutput=useMemo(()=>{
        let output=0;
    for(let i=0; i < 500000000; i++){
        output++
    }

    return output
    },[])
    


    return(
        <div className="App">
            <h2> Parent Component : {`${childSampleOutput} - ${counter}`} </h2>
            <input type="text" value={inputValue} onChange={updateInputValue}/>
            <button onClick={updateCounter}> Update counter value </button>
            <Child counter={counter} changeNumber={updateCounter}/>
        </div>
    )
}

export default Parent;
```

#### Lazy loading for performance
```react
  import { useState } from "react"
import { lazy, Suspense } from "react"

export default function Parent(){
    const [counter,setCounter]=useState<any>(0)

    const LazyComponent:any=lazy(()=> import("./Child"))
    return(
        <>
            <button onClick={()=>setCounter(counter+1)}> Increment </button><h2> {counter} </h2> <button onClick={()=>setCounter(counter-1)}> Decrement </button>
            <Suspense fallback={<div>  Loading Component... </div>} >
            {counter == 5 && <LazyComponent />}   
            </Suspense>
        </>
        
    )
}
```
