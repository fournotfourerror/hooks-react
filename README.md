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
