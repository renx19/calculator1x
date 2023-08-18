

<script lang="ts">

let numbers = [];
let history_list = [];
let result = 0;
let history_text = '';
let is_full_history = false;

function addDecimal() {
    numbers.push('.');

}

function list_to_num(list) {
    let result = '';
    for (let x = 0; x < list.length; x++) {
        result = result + list[x].toString();
    }
    return result;
}

function list_toresult(list) {
    let first = [];
    let second = [];
    let operatorFound = false;
    let operator = '';

    // Find the operator and split the list into two operands
    for (let x = 0; x < list.length; x++) {
        if (list[x] === "+" || list[x] === "-" || list[x] === "x" || list[x] === "/") {
            // Check if an operator is valid based on its position
            if (x === 0 || operatorFound) {
                continue; // Skip invalid operator
            }
            operatorFound = true;
            operator = list[x];
            first = list.slice(0, x);
            second = list.slice(x + 1, list.length);
            break;
        }
    }

    // Convert operands from array to string
    let f_n = list_to_num(first);
    let s_n = list_to_num(second);

    // Perform the operation based on the operator
    switch (operator) {
        case "+":
            result = parseFloat(f_n) + parseFloat(s_n);
            break;
        case "-":
            result = parseFloat(f_n) - parseFloat(s_n);
            break;
        case "x":
            result = parseFloat(f_n) * parseFloat(s_n);
            break;
        case "/":
            result = parseFloat(f_n) / parseFloat(s_n);
            break;
        default:
            result = 0; // Default value if no operator is found
    }

    // Update history_list and history_text
    history_list.push(list_to_num(list) + "=" + result);
    history_text = history_list[history_list.length - 1];
}

function clearHistory() {
    history_list = [];
    history_text = '';
    numbers = [];
    result = 0;
}

</script>
    



 
<main>
    <div class="m1">
        <h1>Calculator</h1>
    </div>
    {#if is_full_history == false}    
    <div class="m2">
        <div class="screen">
            
            {#each numbers as rl}
            <p>{rl}</p>
            {/each}
            <p>{history_text}</p>
            
        </div>
        <div class="buttons ">
            
            <button class="bt" on:click="{()=>{numbers[numbers.length] = "+"}}">+</button>
            <button class="bt" on:click="{()=>{numbers[numbers.length] = "-"}}">-</button>
            <button class="bt" on:click="{()=>{numbers[numbers.length] = "x"}}">x</button>
            <button class="bt" on:click="{()=>{numbers[numbers.length] = "/"}}">/</button>
                <button on:click="{()=>{numbers[numbers.length] = 7}}">7</button>
                <button on:click="{()=>{numbers[numbers.length] = 8}}">8</button>
                <button on:click="{()=>{numbers[numbers.length] = 9}}">9</button>
                <button class="bt" on:click="{()=>{list_toresult(numbers);numbers= [];}}">=</button>
                <button on:click="{()=>{numbers[numbers.length] = 4}}">4</button>
                <button on:click="{()=>{numbers[numbers.length] = 5}}">5</button>
                <button on:click="{()=>{numbers[numbers.length] = 6}}">6</button>
                <button on:click={() => addDecimal()}>.</button>
                <button on:click="{()=>{numbers[numbers.length] = 1}}">1</button>
                <button on:click="{()=>{numbers[numbers.length] = 2}}">2</button>
                <button on:click="{()=>{numbers[numbers.length] = 3}}">3</button>
                <button class ="clr" on:click="{()=>{numbers= []; history_text="",result=0;}}">Clear</button>
                <button on:click="{()=>{numbers[numbers.length] = 0}}">0</button>
            
        </div>
        <div class="history">
            <div>
                <button on:click="{()=>{is_full_history = true;}}">Show History</button>
            </div>
            {#each history_list.reverse().slice(0,7) as hl}
                <p>{hl}</p>
            {/each}
        </div>
    </div>
    {/if}
    {#if is_full_history == true}


    
        
    <div class="m3">
        <div class="m3_head">
 
            <button on:click="{()=>{is_full_history = false;}}">X</button>
        </div>
        <div class="m3_body">

            {#each history_list.reverse().slice(0,45) as hl}
                <p>{hl}</p>
            {/each}
        </div> 
        <div class="m3_footer">
            <button class="clr" on:click="{() => { clearHistory(); }}">Delete History</button>
            
          
        </div>
    </div>
    {/if}

</main>
<style>
:root{
    --font-s1:36px;
    --font-s2:22px;
    --font-s3:18px;
    --font-s4:16px;
    --font-c:#3b3e3e;
    --bg-c: #b8b8b8 ;
    --number-bg-color: black;
    --number-text-color: white;
    --operation-bg-color: #FF8C00;
    --operation-text-color: white;
    --delete-clear-bg-color: #8B0000;
    --delete-clear-text-color: white;
  }

  main{
    display: grid;
    place-content: center;
    
}
main>div{
    font-size: var(--font-s1);
    color: var(--font-c);
}
.m1{
    display:grid;
    place-content: center;
}
.m2{
    display: grid;
    grid-template-columns: 1fr 1fr;
    grid-template-rows: .3fr 1fr;
    gap:1rem;

}
.m3{
    display:grid;
    grid-template-columns: 500px;
    grid-template-rows: 30px 1fr;
    height:auto;
    background-color: rgb(255, 255, 255);
    border-radius: 3rem;
    padding: 2rem;
    border:1px double black;
    animation-name: show;
    animation-duration:1000ms;
}

.m3_body{
    grid-column: 1/2;
    grid-row: 2/3;
    display: grid;
    grid-template-columns: repeat(3,1fr);
    grid-template-rows: repeat(5,1fr);
    gap:1rem;
}
.m3_body>p{
    font-size: var(--font-s3);
    padding: 2px;
    border-radius: 1rem;
    background-color: #33b3b3;
    color:white;
    font-weight: 600;
    display:grid;
    place-content:center;
}
.m3_body>p:hover{
    background-color: #3b3e3e

}
.m3_head{
    grid-column: 1/2;
    grid-row: 1/2;
    display: flex;
    justify-content: end;
}
.m3_head>button{
    background-color: white;
    border: 0px;
    font-size: var(--font-s2);
    height: 100%;
    width:30px;
    padding: 2px;
    border-radius: 3rem;
    display:grid;
    place-content: center;
    cursor:pointer;
}

.bt{
    font-weight: 600;
    background-color: var(--operation-bg-color) !important;
    color: var(--operation-text-color) !important;
}

.clr{
  
    font-weight: 600;
    background-color: var(--delete-clear-bg-color) !important;
    color: var(--delete-clear-text-color) !important;
    padding: 10px;
    border-radius: 16px; 
}

.clr:hover{
    background-color: #3b3e3e !important;
}
.history{
    grid-column: 2/3;
    grid-row: 1/-1;
    display:flex;
    flex-direction: column;
    justify-content: start;
    align-items: center;
    font-size: var(--font-s3);
    color:var(--font-c);
}
.history>div{
    display:flex;
    justify-content: space-between;
    align-items: center;
}
.history>div>button{
    cursor: pointer;
    margin-inline: 16px;  
  padding-inline: 28px;
  padding-block: 12px; 
  border-radius: 16px; 
    border:1px solid black ;
    background-color: rgb(47, 237, 231)
}
.history>div>button:hover{
    animation-name: scale_it;
    animation-duration: 300ms;
    animation-fill-mode: forwards;
    background-color: rgb(149, 163, 165);
}
.history>p{
    animation-name: show;
    animation-duration:1000ms;
}

@keyframes show{
    0%{
        opacity:0%
    }
    100%{
        opacity:100%
    }
}

.screen{
    grid-column: 1/2;
    grid-row: 1/2;
    border: 1px solid var(--bg-c);
    display:flex;
    justify-content: center;
}
.screen>p{
    animation-name: show;
    animation-duration:800ms;
}
.buttons{
    grid-column: 1/2;
    grid-row: 2/3;
    display: grid;
    place-content: center;
    grid-template-columns: repeat(4, 65px);
    grid-template-rows: repeat(6,50px);
    gap:1rem;
    
}
.buttons>button{
   
    display:flex;
    justify-content: center;
    align-items: center;
    border: 0px;
    font-size: var(--font-s2);
    color: var(--number-text-color);
    background-color:var(--number-bg-color);
   
}
.buttons>button:hover{
    
    background-color: var(--bg-c);
    color: white;
    animation-name: scale_it;
    animation-duration: 300ms;
    animation-fill-mode: forwards;
}
@keyframes scale_it{
    0%{
        transform:scale(100%);
        border-radius: 1rem;
    }
    100%{
        
        transform:scale(110%);
        border-radius: 2rem;
    }

}

@media screen and (max-width:676px){
.m2{
    display: grid;
    grid-template-columns: 1fr ;
    grid-template-rows: .3fr 1fr 1fr;
    gap:1rem;
}
.history{
    grid-column: 1/2;
    grid-row: 3/4;
   
}

}
</style>

