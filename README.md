# voxnode-clr

Utilice ensamblados y compile código .NET (Framework 4 , Mono) en nodejs
voxnode-clr permite usar en nodejs [vox-core-clr](https://www.npmjs.com/package/vox-core-clr) sin necesidad de instalar [vox-core](https://www.npmjs.com/package/vox-core)


### Instalar

```sh
> $ npm install voxnode-clr
```

### Documentación 

Solo tiene que hacer un require al módulo:

```javascript
require("voxnode-clr");
```

Después utilice el módulo según las instrucciones de [vox-core-clr](https://www.npmjs.com/package/vox-core-clr)


### NOTA

Los bugs deben hacerse en el git de [vox-core](https://github.com/voxsoftware/vox-core/), ya que voxnode-clr funciona usando vox-core-clr, y los cambios que se hagan se harán sobre vox-core-clr


### Ejemplo

Este es el mismo ejemplo tomado de vox-core-clr pero código transpilado a ES5 manualmente:

```javascript
require("voxnode-clr") 
var clr= new core.VW.Clr.Manager()

var awaitTask= function(task, func){
	if(task && task.then) // core.VW.Task
		task.then(func, func)
	else
		return func()
}

var test=function(){
	var step= 0, d
	var context={
		current: null,
		value:null
	}
	var call= function(){

		if(step==-1){
			process.exit(0)
		}

		if(context.current){
			if(context.current.exception){
				console.error(context.current.exception)
				step= -1
				return call()
			}
			context.value=context.current.result
			
		}
		switch(step){
			case 0:
				context.current=clr.loadAssembly("System.Xml")
				step= 1
				awaitTask(context.current, call)
				break;
			case 1:
				d= new Date()
				context.current= testxml()
				step= 2
				awaitTask(context.current, call)
				break;
			case 2:
				console.info("Time: ", new Date()-d)
				d= new Date()
				context.current= testxml()
				step= 3
				awaitTask(context.current, call)
				break;
			case 3: 
				console.info("Time: ", new Date()-d)
				d= new Date()
				context.current= testxmlScope()
				step= 4
				awaitTask(context.current, call)
				break;

			case 4: 
				console.info("Time: ", new Date()-d)
				d= new Date()
				context.current= testxmlScope()
				step= 5
				awaitTask(context.current, call)
				break;

			case 5: 
				context.current= clr.close()
				step= 6
				awaitTask(context.current, call)
				break;
			case 6: 
				console.info("Time: ", new Date()-d)
				step= -1
				call()
				break;
		}
	}
	call()
}




var testxml= function(){
	var step= 0, Xml={}, elemento1, elemento2, doc, root, task
	var context={
		current: null,
		value:null
	}

	task= new core.VW.Task()
	var call= function(){

		if(step==-1){
			return task.finish()
		}

		if(context.current){
			if(context.current.exception){
				console.error(context.current.exception)
				step= -1
				return call()
			}
			context.value=context.current.result
			
		}
		switch(step){
			case 0:
				Xml.Document= clr.get("System.Xml.XmlDocument")
				context.current= Xml.Document.loadMembers()
				step= 1
				awaitTask(context.current, call)
				break;
			case 1:
				context.current= Xml.Document.create()
				step= 2
				awaitTask(context.current, call)
				break;
			case 2: 
				doc= context.value
				context.current= doc.CreateXmlDeclaration("1.0","utf8","yes")
				step= 3
				awaitTask(context.current, call)
				break;

			case 3: 
				root= context.value
				context.current= doc.AppendChild(root)
				step= 4
				awaitTask(context.current, call)
				break;

			case 4: 
				context.current= doc.CreateElement("element1")
				step= 5
				awaitTask(context.current, call)
				break;
			case 5: 
				elemento1= context.value
				context.current= doc.CreateElement("element2")
				step= 6
				awaitTask(context.current, call)
				break;

			case 6: 
				elemento2= context.value
				context.current= elemento1.AppendChild(elemento2)
				step= 7
				awaitTask(context.current, call)
				break;

			case 7: 
				context.current= elemento2.setInnerText("Hola mundo!")
				step= 8
				awaitTask(context.current, call)
				break;

			case 8: 
				context.current= doc.AppendChild(elemento1)
				step= 9
				awaitTask(context.current, call)
				break;

			case 9: 
				context.current= doc.getOuterXml()
				step= 10
				awaitTask(context.current, call)
				break;
			case 10: 
				console.info(context.value)
				var tasks=[root.dispose(), elemento1.dispose(),
				elemento2.dispose(), doc.dispose()]

				context.current= core.VW.Task.waitMany(tasks)
				step= -1
				awaitTask(context.current, call)
				break;
		}
	}
	call()
	return task
}



var testxmlScope= function(){
	var step= 0, Xml={}, elemento1, elemento2, doc, root, scope, task
	var context={
		current: null,
		value:null
	}

	task= new core.VW.Task()
	var call= function(){

		if(step==-1){
			return task.finish()
		}

		if(context.current){
			if(context.current.exception){
				console.error(context.current.exception)
				step= -1
				return call()
			}
			context.value=context.current.result
			
		}
		switch(step){
			case 0:
				scope= clr.beginScope()
				Xml.Document= clr.get("System.Xml.XmlDocument")
				context.current= Xml.Document.loadMembers()
				step= 1
				awaitTask(context.current, call)
				break;
			case 1:
				context.current= scope(Xml.Document).create()
				step= 2
				awaitTask(context.current, call)
				break;
			case 2: 
				doc= context.value
				context.current= doc.CreateXmlDeclaration("1.0","utf8","yes")
				step= 3
				awaitTask(context.current, call)
				break;

			case 3: 
				root= context.value
				context.current= doc.AppendChild(root)
				step= 4
				awaitTask(context.current, call)
				break;

			case 4: 
				context.current= doc.CreateElement("element1")
				step= 5
				awaitTask(context.current, call)
				break;
			case 5: 
				elemento1= context.value
				context.current= doc.CreateElement("element2")
				step= 6
				awaitTask(context.current, call)
				break;

			case 6: 
				elemento2= context.value
				context.current= elemento1.AppendChild(elemento2)
				step= 7
				awaitTask(context.current, call)
				break;

			case 7: 
				context.current= elemento2.setInnerText("Hola mundo!")
				step= 8
				awaitTask(context.current, call)
				break;

			case 8: 
				context.current= doc.AppendChild(elemento1)
				step= 9
				awaitTask(context.current, call)
				break;

			case 9: 
				context.current= doc.getOuterXml()
				step= 10
				awaitTask(context.current, call)
				break;
			case 10: 
				console.info(context.value)
				context.current= scope.end()
				step= -1
				awaitTask(context.current, call)
				break;
		}
	}
	call()
	return task
}

test()
```