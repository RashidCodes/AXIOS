## MAKING BASIC HTTP REQUESTS WITH AXIOS

Web tokens can be stored in the header like so...
```bash
axios.defaults.headers.common['X-Auth-Token'] = 'lkafasgarrhaerabaqewgavaeadvadv';
```

### AXIOS INSTANCES
```bash
const axiosInstance = axios.create({
    baseURL : 'https://jsonplaceholder.typicode.com'
})

axiosInstance.get('/comments').then(res => showOutput(res))
```


### GET REQUESTS
```bash
axios({
    method: 'get',
    url: 'https:sonplaceholder.typicode.com/todos',
    params: {
        _limit: 5
    }
}).then(res => showOutput(res))
.catch(err => console.log(err))



OR 

axios
.get('https://jsonplaceholder.typicode.com/todos?_limit=5')
.then(res => showOutput(res))
.catch(err => console.log(err))


OR
axios('https://jsonplaceholder.typicode.com/todos?_limit=5')
.then(res => showOutput(res))
.catch(err => console.log(err))
```

Just in case you want to set a timeout

```bash
axios('https://jsonplaceholder.typicode.com/todos?_limit=5', {
    timeout: 5 // in milliseconds
})
.then(res => showOutput(res))
.catch(err => console.log(err))
```

### POST REQUEST
```bash
axios({
    method : 'post',
    url: 'https://jsonplaceholder.typicode.com/todos',
    data: {
        title: 'New Todo',
        completed: false 
    }
}).then(res => showOutput(res))
.catch(err => console.log(err))


OR 


axios.post('https://jsonplaceholder.typicode.com/todos', {
    title : 'Accra',
    completed: false
}).then(res => showOutput(res))
.catch(err => console.log(err))
```


### PUT OR PATCH REQUEST
```bash
axios.patch('https://jsonplaceholder.typicode.com/todos/1', {
    title : 'Patched todo',
    completed: true
}).then(res => showOutput(res))
.catch(err => console.log(err))


axios.put('https://jsonplaceholder.typicode.com/todos/1', {
    title : 'Updated todo',
    completed: true
}).then(res => showOutput(res))
.catch(err => console.log(err))  
```


### DELETE REQUEST
```bash
axios.delete('https://jsonplaceholder.typicode.com/todos/1')
.then(res => showOutput(res))
.catch(err => console.log(err))
```


### MULTIPLE REQUESTS
```bash
axios.all([
    axios.get('https://jsonplaceholder.typicode.com/todos?_limit=5'),
    axios.get('https://jsonplaceholder.typicode.com/posts?_limit=5')
]).then(res => {
    console.log(res[0])
    console.log(res[1])
    showOutput(res[1])
}).catch(err => console.log(err))


better syntax ( axios.spread() )
axios.all([
    axios.get('https://jsonplaceholder.typicode.com/todos?_limit=5'),
    axios.get('https://jsonplaceholder.typicode.com/posts?_limit=5')
]).then(axios.spread((todos, posts) => showOutput(posts)))
.catch(err => console.error(err))
```



## CUSTOM HEADERS
```bash
const config = {
    headers: {
        'Content-Type' : 'application/json', 
        Authorization: 'sometoken'
    }
}

axios.post('https://jsonplaceholder.typicode.com/todos', 
{
    title: 'New Todo',
    completed: false
}, 
config)
.then(res => showOutput(res))
.catch(err => console.error(err))
```


### TRANSFORMING REQUESTS AND RESPONSES
```bash
const options = {
    method: 'post',
    url: 'https://jsonplaceholder.typicode.com/todos?_limit=5',
    data : {
        title: 'Hello World'
    },
    transformResponse: axios.defaults.transformResponse.concat(data => {
        data.title = data.title.toUpperCase()
        return data
    })
}

axios(options).then(res => showOutput(res))
```


### ERROR HANDLING
```bash
axios('https://jsonplaceholder.typicode.com/todoss?_limit=5', {
    validateStatus: function(status){
        return status < 500 // reject only if statuus is greater than or equal to 500
    }
})
.then(res => showOutput(res))
.catch(err => {
    if(err.response){
        console.log(err.response.data)
        console.log(err.response.status)
        console.log(err.response.headers)
    }

    if(err.response.status === 404){
        alert('Error: Page not found')
    }

    if(err.request){
        // Request was made but there was no response
        console.log(err.request)
    } else {
        console.log(err.message)
    }
})
```




### CANCEL TOKEN
```bash
const source = axios.CancelToken.source();

axios('https://jsonplaceholder.typicode.com/todos?_limit=5', {
    cancelToken: source.token
})
.then(res => showOutput(res))
.catch(thrown => {
    if(axios.isCancel(thrown)){
        console.log('Request cancelled', thrown.message)
    }
})

// if for some reason you want to cancel the request
if(true){
    source.cancel('Request cancelled')
}
```



### INTERCEPTING REQUETS AND RESPONSES
Basically, log requests to the console.
```bash
axios.interceptors.request.use(config => {
    console.log(`${config.method.toUpperCase()} request sent to ${config.url} at ${new Date().getTime()}`)
    return config 
}, error => {
    return Promise.reject(error)
})
```