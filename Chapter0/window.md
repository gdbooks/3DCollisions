#Window

Making a windowed sample is as easy as making a console one. If you want a window, you probably don't need to override the ```Main``` method, but you will need to override one or more of the following:

* Intialize(int width, int height)
* Resize(int width, int height)
* Update(float deltaTime)
* Render()
* Shutdown()