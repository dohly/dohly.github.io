Mix Task and Async easy!
========================

.. code-block:: fsharp
   
      open System.Threading.Tasks

      type TaskAndAsync()=
          member __.ReturnFrom(x:Task)=Async.AwaitTask x
          member __.ReturnFrom(x:Task<_>)=Async.AwaitTask x
          member __.ReturnFrom(x:Async<_>)=x

          member __.Bind(x:Task, f)=async.Bind(Async.AwaitTask x, f)
          member __.Bind(x:Task<_>, f)=async.Bind(Async.AwaitTask x, f)
          member __.Bind(x:Async<_>, f)=async.Bind(x, f)

          member __.Zero()=async.Zero()
      
      let taskAndAsync=TaskAndAsync()
      let result=
            taskAndAsync {
                do! Async.Sleep 1000 // Async<_>
                do! Task.Delay 100 // Task
                return! Task.FromResult 1 // Task<>
            }