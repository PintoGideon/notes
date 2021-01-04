### State Management with Hooks

Lazy State initialization

```javascript



function Greeting(){
    console.log('rendering')
    function getinitialNameValue(){
        return window.localStorage.getItem('name')|| ''
    }

    const [name, setName]=React.useState(getInitialNameValue)

    React.useEffect(()=>{
        window.localStorage.setItem('name',name)
    })
    return(
        <div>


        </div>
    )
}



```