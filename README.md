# MultiTask
FreePascal Multitask library

Very simple to use MultiTask library for FreePascal.

<b>Some properties :</b>
- Simple<br/>
- FPC Windows/Linux compatible<br/>
- Add task on run - you can enqueue task anytime<br/>
- Priorities of task - you can add task priority - that's not thread priority, but place in the TaskQueue - tpFirst,tpHigh,tpNormal,tpLow,tpLast (default)<br/>
- Priorities can be globaly disabled<br/>
- Tasks is normal Threads<br/>


<b>Some properties (planed, in near future) :</b>
- Pin Threads to cores<br/>
- Before build method compilation - aplication which will build On_New_Work, and some methods directly <br/>
 


<b>Initialization of library :</b>

  MultiTask := tMultiTask.Create;<br/>
  MultiTask.On_Task_Run_Method:=@On_New_Work;    // main method of set parameters to NORMAL methods/procedures<br/>
  MultiTask.On_Before_Task_Method:=@__On_Before_Task_Method;<br/>
  MultiTask.On_After_Task_Method:=@__On_After_Task_Method;<br/>


<b>Finalization :</b>

  MultiTask.Free;<br/>


<b>Main Work :</b>

  <add tasks to queue possible here><br/>
  MultiTask.Start;<br/>
  <add another tasks to queue possible here><br/>
  MultiTask.WaitFor;<br/>

<b>Example of On_New_Work method :</b>
procedure tmyObject.On_New_Work(const method_name: string; const task: tMultiTaskItem);<br/>
begin<br/>
   case method_name of<br/>
     'load_image' : Load_Image(task.I[1],task.S[2]);<br/>
     'save_image' : Save_Image(task.I[1],task.S[2],tBGRABitmap(task.O[3]));<br/>
   end; <br/>
end;<br/>


<b>Enqueue task to queue :</b>

for enqueue method :<br/>

procedure tMyObject.Load_Image(const id : integer; const fn: string);<br/>
var img : tBGRABitmap;<br/>
begin<br/>
   img := tBGRABitmap.Create(fn);<br/>
   MultiTask.Enqueue(tTaskMethod(@Save_Image),[id,fn,img],[tpHigh]); // enqueue Load_Image method with param ID and filename with HighPriority<br/>
end;<br/>

use this :<br/>

MultiTask.Enqueue(tTaskMethod(@Load_Image),[id,fn],[tpHigh]); // enqueue Load_Image method with param ID and filename with HighPriority<br/>


 <b>What is :</b>
 
 MultiTask - main class for automated running any task<br/>
 Task - simple method or procedure, simply doing some work - as normal method or procedure<br/>
 
 <b>What to see :</b>
 
 Memory managment - When you enqeueue task with some objects as parameters, do not free this objects BEFORE task end - simple free this object near task end<br/>
 
 
<b>Tested on :</b>
Ubuntu 16.10 - crosscompile from Windows, FPC 3.0.0, Lazarus 1.6.3<br/>
Ubuntu 17.04 - crosscompile from Windows, FPC 3.0.0, Lazarus 1.6.3<br/>
Windows 8.1 - partial test, compiled on Windows, FPC 3.0.0, Lazarus 1.6.3<br/>
 
