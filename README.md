Closures by Lizette Rodriguez

**¿What is a closure? 
Closures are functions that can access the lexical environment of their parent function. This means that they're nested. An identifying characteristic is that they keep the current value of the variables in the parent lexical environment even after they are done executing. 

**Give me an example of closure. 
function PostOffice(){
  var MsgCount=0;
  function SendMsg(msg){
    let msgsent="Message: "+msg;
    MsgCount++
    let Counter=MsgCount+" Messages have been sent"
    return msgsent+"\n"+Counter
  }
  return SendMsg
}
const ScrantonOffice=PostOffice();
console.log(ScrantonOffice("Hi, Liz!"))//Message: Hi, Liz! 1 Messages have been sent
console.log(ScrantonOffice("Hi, Angel!"))//Message: Hi, Angel! 2 Messages have been sent

**What is ()() in code?
double parenthesis makes the nested functions run. Therefore both the outer and inner functions are invoked.
PostOffice()("Hi, Liz!");//Message: Hi, Liz! 1 Messages have been sent

**Move the variable after the closure (the function inside the function) and explain what happens.
The process works because the invocation of the functions happens after the declaration of the variable despite it being after the closure. Yet, it needs to be before the return because this stops the function so everything after does not run.

function PostOffice(){
  function SendMsg(msg){
    let msgsent="Message: "+msg;
    MsgCount++
    let Counter=MsgCount+" Messages have been sent"
    return msgsent+"\n"+Counter
  }
   var MsgCount=0;
  return SendMsg
}
const ScrantonOffice=PostOffice();
console.log(ScrantonOffice("Hi, Liz!"))//Message: Hi, Liz! 1 Messages have been sent
console.log(ScrantonOffice("Hi, Angel!"))//Message: Hi, Angel! 2 Messages have been sent

**Change var for let and explain why the logic is not affected
The function still works because of the invocation placement despite the type of variable. Hoisting is not involved.
function PostOffice(){
  function SendMsg(msg){
    let msgsent="Message: "+msg;
    MsgCount++
    let Counter=MsgCount+" Messages have been sent"
    return msgsent+"\n"+Counter
  }
   let MsgCount=0;
  return SendMsg
}

**Scope chain, an example of it, how many closures can we nest.
Each nested function has a link to the lexical environment of its parent.
function outer (name){
  let a="Welcome to JS closures, ";
  function inner(){
    let str=a+"hope you enjoy it, ";
    function inner2(){
      let msg=str+name+"!";
      return msg
    }
    return inner2;
  }
  return inner
}
const welcome1=outer("Liz")();
const welcome2=outer("Angel")();
console.log(welcome1())//Welcome to JS closures, hope you enjoy it, Liz!
console.log(welcome2())//Welcome to JS closures, hope you enjoy it, Angel!

**Are there conflicts between the closure and the global scope? 
Due to the scope of each variable, there is no conflict. The var inside the function is accessed only when the function is executed because it is directly found in its lexical environmenta and it's encapsulated; while the global var is only accesible in the global scope, so when console logged the var showed is the global one.
var SentCount= 20;

function PostOffice(){
  var SentCount=0;
  var ReceivedCount=0;
  this.SendMsg = (msg) => {
    let msgsent="Message: "+msg;
    SentCount++
    let Counter=SentCount+" Messages have been sent"
    console.log(msgsent+"\n"+Counter)
  }
  this.ReceiveMsg = (from) => {
    let sender="Message from: "+from;
    ReceivedCount++
    let Counter=ReceivedCount+" Messages have been received"
    console.log(sender+"\n"+Counter)
  }
}
const ScrantonOffice=new PostOffice();
const NewYorkOffice=new PostOffice();
ScrantonOffice.SendMsg("Hi, Liz!");
ScrantonOffice.ReceiveMsg("Pam");
NewYorkOffice.SendMsg("Hi, Rachel!");
NewYorkOffice.ReceiveMsg("Monica");
console.log(SentCount);

Advantages of closures.
Some of the advantages are: 
1. Data hiding for privacy and avoiding external access
2. Factory functions, where different instances of a function can be created that act on their own values
3. One can have variables with the same name in global and inside the function because the closure provides encapsulating and whatever happens inside the function does not affect the global variable.

**What is data hiding and encapsulation?
Data hiding is making variables unaccessible from the exterior of an object this makes them private and unmodifyable from the outside of the object or in this case function. Encapsulating is through what variables are wrapped, in a closure the variables in the parent are encapsulated because they cannot be modified by any external code and is only accessible by the inner/child function.

**Give me an example of privacy with closures. 
Here all 3 variables SentCount, ReceivedCount and ClientID are private because the exterior cannot corrupt them. This kind of information can be sensitive for keeping track of the operations handled in a post office and who made which operation.
function PostOffice(){
  var SentCount=0;
  var ReceivedCount=0;
  var ClientID=0;
  this.SendMsg = (msg) => {
    let msgsent="Message: "+msg;
    SentCount++;
    ClientID++;
    let Counter=SentCount+" Messages have been sent"
    console.log(msgsent+"\n"+Counter+"\n"+"You're the #"+ClientID+" client")
  }
  this.ReceiveMsg = (from) => {
    let sender="Message from: "+from;
    ReceivedCount++
    ClientID++;
    let Counter=ReceivedCount+" Messages have been received"
    console.log(sender+"\n"+Counter+"\n"+"You're the #"+ClientID+" client")
  }
}
const ScrantonOffice=new PostOffice();
const NewYorkOffice=new PostOffice();
ScrantonOffice.SendMsg("Hi, Liz!");
ScrantonOffice.ReceiveMsg("Pam");

NewYorkOffice.SendMsg("Hi, Rachel!");
NewYorkOffice.ReceiveMsg("Monica");

Output:
Message: Hi, Liz!
1 Messages have been sent
You're the #1 client
Message from: Pam
1 Messages have been received
You're the #2 client

Message: Hi, Rachel!
1 Messages have been sent
You're the #1 client
Message from: Monica
1 Messages have been received
You're the #2 client

**What happens if you create two counters with the same closure? 
Every time a function is invoked to be executed a new execution context is created with its own new lexical environment, thus, each one has its own count. The newest one will act as if the function was reset.
function PostOffice(){
  let MsgCount=0;
  function SendMsg(msg){
    let msgsent="Message: "+msg;
    MsgCount++
    let Counter=MsgCount+" Messages have been sent"
    return msgsent+"\n"+Counter
  }
  return SendMsg
}
const ScrantonOffice=PostOffice();
const NewYorkOffice=PostOffice();
console.log(ScrantonOffice("Hi, Liz!"))//Message: Hi, Liz! 1 Messages have been sent
console.log(ScrantonOffice("Hi, Angel!"))//Message: Hi, Angel! 2 Messages have been sent
console.log(NewYorkOffice("Hi, Rachel!"))//Message: Hi, Rachel! 1 Messages have been sent
console.log(NewYorkOffice("Hi, Ross!"))//Message: Hi, Ross! 2 Messages have been sent

**How can we add more functions as a decrement counter? Give an example of it.  
Each function can have more than 1 nested function. They can be set as properties so later they can be called as a method of the function like such: ScrantonOffice.SendMsg("Hi, Liz!");

function PostOffice(){
  let SentCount=0;
  let ReceivedCount=0;
  this.SendMsg = (msg) => {
    let msgsent="Message: "+msg;
    SentCount++
    let Counter=SentCount+" Messages have been sent"
    console.log(msgsent+"\n"+Counter)
  }
  this.ReceiveMsg = (from) => {
    let sender="Message from: "+from;
    ReceivedCount++
    let Counter=ReceivedCount+" Messages have been received"
    console.log(sender+"\n"+Counter)
  }
}
const ScrantonOffice=new PostOffice();
const NewYorkOffice=new PostOffice();
ScrantonOffice.SendMsg("Hi, Liz!");//Message: Hi, Liz! 1 Messages have been sent
ScrantonOffice.SendMsg("Hi, Angel!");//Message: Hi, Angel! 2 Messages have been sent
ScrantonOffice.ReceiveMsg("Pam");//Message from: Pam 1 Messages have been received
ScrantonOffice.ReceiveMsg("Jim");//Message from: Jim 1 Messages have been received

NewYorkOffice.SendMsg("Hi, Rachel!");//Message: Hi, Rachel! 1 Messages have been sent
NewYorkOffice.SendMsg("Hi, Ross!");//Message: Hi, Ross! 2 Messages have been sent
NewYorkOffice.ReceiveMsg("Monica");//Message from: Monica 1 Messages have been received
NewYorkOffice.ReceiveMsg("Joey");//Message from: Joey 2 Messages have been received

**What are the disadvantages of closures? 
A disadvantage is that while closures are active they cannot be collected by the garbage collector and therefore occupy space in the memory when unnecessary, this can lead to memory leak.
