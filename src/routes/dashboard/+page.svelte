<script>
    import { db } from "../../lib/firebase/firebase";
    import { authHandlers, authStore } from "../../store/store";
    import { getDoc, doc, setDoc } from "firebase/firestore";

  


  let todoList =[];
  let currentToDo = "";
  let error = false;

  function addToDo(){
    error= false;
    if(!currentToDo){
      error=true;
    }
    todoList=[...todoList, currentToDo];
    currentToDo = ""
  }

  // @ts-ignore
  function editTodo(index){

    // @ts-ignore
    let newTodoList = todoList.filter((val,i) =>{
      return i !== index;
    })
    currentToDo = todoList[index];
    
    todoList = newTodoList;
  }

  // @ts-ignore
  function deleteTodo(index){
    let newTodoList = todoList.filter((val,i) =>{
      return i !== index;
    })
    
    
    todoList = newTodoList;

  }

  async function saveTodos() {
        try {
            const userRef = doc(db, "users", $authStore.user.uid);
            await setDoc(
                userRef,
                {
                    todos: todoList,
                },
                { merge: true }
            );
        } catch (err) {
            console.log("There was an error saving your information");
        }
    }
</script>



<div class="mainContainer">
  <div class="headerConatiner">
    <h1>Todo List</h1>
    <div class=" headerBtn">
      <button on:click={saveTodos}>
        <i class="fa-regular fa-floppy-disk"/>
        Save
      </button>

      <button on:click={authHandlers.logout}>
        <i class="fa-solid fa-right-from-bracket"/>
        LogOut
      </button>
    </div>
  </div>
  <main>
    {#if todoList.length === 0}
    <p>You have nothing to do!!</p>
    {/if}
    {#each todoList as todo , index}
       <div class="todo">
        <p>
          {index + 1}.{todo}
        </p>
        <div class="actions">
          <i on:click={()=>{
            editTodo(index)

          }} on:keydown={() =>{}} class="fa-regular fa-pen-to-square"/>


          <i on:click={()=>{
            deleteTodo(index)

          }} on:keydown={() =>{}}  class="fa-regular fa-trash-can"/>

        </div>

       </div>

    {/each}

  </main>
  <div class={"enterTodo" + (error ? "errorBorder" : "")}>
    <input bind:value={currentToDo} type="text" placeholder="Enter todo"/>
    <button on:click={addToDo}>Add</button>
  </div>

</div>

<style>
  .mainContainer{
    display: flex;
    flex-direction: column;
    min-height: 100vh;
    gap:24px;
    padding: 24px;
    width: 100%;
    max-width:1000px;
    margin:0 auto


  }
  .headerBtn {
    display: flex;
    align-items: center;
    gap:8px;

  }



  .headerConatiner{
    display: flex;
    justify-content: space-between;
    align-items: center;
    
  }

  .headerConatiner button{
    background:gray;
    color:white;
    padding:10px 18px;
    font-family: "Josefin Sans", sans-serif;
    border: none;
    border-radius: 4px;
    font-weight: 600;
    display: flex;
    align-items: center;
    gap:8px
  }
  .headerConatiner button:hover{
    background:darkgray;
    color: black;
    cursor: pointer;
  }
  main{
  display: flex;
  flex-direction: column;
  gap:10px;
  flex:1;

  }
  .todo{
    border-left: 1px solid cyan;
    padding:8px 15px;
    display: flex;
    justify-content: space-between;
    align-items: center;

  }
  .actions{
    display: flex;
    align-items: center;
    gap: 14px;
    font-size: 1.3rem;
  }

  .actions i:hover{
    cursor: pointer;
    color: orange;
  }
  .enterTodo{
    display: flex;
    align-items: stretch;
    border:1px solid #fff;
    border-radius:5px ;
    overflow: hidden;
    
  }

  .errorBorder{
    border-color: rgb(220, 126, 18) !important;
  }

  .enterTodo input{
    background: transparent;
    border: none;
    padding: 15px;
    color: #fff;
    flex:1;
  }
  .enterTodo input:focus{
    border: none;
    border: 1px solid rgb(61, 163, 204);
  }
  .enterTodo button{
    padding: 0 15px;
    background:#003c5b;
    font-weight: 600;
    cursor: pointer;
    color: #fff;
  }

  .enterTodo button:hover{
    background: transparent;
  }


</style>