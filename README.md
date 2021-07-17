 
 ## Hydralit Example<img src="./docs/images/hydra.png" alt="drawing" width="50"/>
The Hydralit package is a wrapping and template project to combine multiple independant (or somewhat dependant) Streamlit applications into a multi-page application.

Currently the project implements a host application HydraApp and each child application simply needs to be a class deriving from the HydraHeadApp class and implement a single, simple method, run().

This is a sample project that tries to show some of the potential of using the Hydralit package to create multi-page Streamlit applications using the existing approach of creating a number of single applications and then using this package to make it easy to create a multi-page application from the individual applications in a simple way.

The ability to add as many applications as you wish and create a dedicated secure login application to front door all the seperate applications, means you can think about your project code and just add it to the host Hydralit application and you have a full-blooded, state aware multi-page application, with security and navigation.

If you find this project useful, please give it a star or atleast a "hey! i find this useful, thanks" callout, I hope to make everyones life alittle easier by using Hydralit.

### You can try it out by installing the project requirements and then running the sample secure app as below.

##### First install the project dependencies using the requirements.txt file, then let rip as below.
```bash
pip install -r requirements.txt
```

## Installation
Hydralit can be installed from PyPI:

```bash
pip install hydralit
```

You can run the sample secure app with the commands below. (dummy login details are, u:joe, p:joe).

```bash
hydralit_example> streamlit run secure_app.py
```

The host application code is shown below as an example of how such a multi-page application with authentication and lots of bells and whistles can be created with very little code, yet alot of configuration potential.

#### secure_app.py
```python
from hydralit import HydraApp
import streamlit as st
import apps


if __name__ == '__main__':

    #------------all valid container references, do not initilise or page config will not be applied (this is from beta containers, beta, who knew!).
    #my_container = st.sidebar
    # my_container = st.sidebar.beta_container
    # my_container = st.sidebar.beta_columns
    #my_container = st
    # my_container = st.beta_container
    # my_container = st.beta_columns
    my_container = None

    #this is the host application, we add children to it and that's it!
    app = HydraApp(title='Secure Hydralit Data Explorer',favicon="🐙",nav_container=my_container,nav_horizontal=False,hide_streamlit_markers=True)
  
    #add all your application classes here
    app.add_app("Cheat Sheet", icon="📚", app=apps.CheatApp())
    app.add_app("Sequency Denoising",icon="🔊", app=apps.WalshApp(title='Walsh Data'))
    app.add_app("Sequency Denoising1",icon="🔊", app=apps.WalshApp(title='Walsh Data'))
    app.add_app("Solar Mach", icon="🛰️", app=apps.SolarMach(title='Solar-MACH'))

    #Home button will be in the middle of the nav list now
    app.add_app("Home", icon="🏠", app=apps.HomeApp(title='Home'),is_home=True) 

    app.add_app("Spacy NLP", icon="⌨️", app=apps.SpacyNLP())
    app.add_app("Uber Pickups", icon="🚖", app=apps.UberNYC())

    #we want to have secure access for this HydraApp, so we provide a login application
    #optional logout label, can be blank for something nicer!

    #app.add_app("Login", apps.LoginApp(title='Login'),is_login=True,logout_label='Piss Off 🖕')
    app.add_app("Login", apps.LoginApp(title='Login'),is_login=True) 

    # If the menu is cluttered, just rearrange it into sections!
    # completely optional, but if you have too many entries, you can make it nicer by using accordian menus
    complex_nav = {
        'Home': ['Home'],
        'Intro 🏆': ['Cheat Sheet',"Solar Mach"],
        'Hotstepper 🔥': ["Sequency Denoising","Sequency Denoising1"],
        'Models 🧩': ["Spacy NLP","Uber Pickups"],
    }
    
    #add a custom loader for app transitions
    #app.add_loader_app(apps.MyLoadingApp())

    #st.write('**This is completely outside of all HydraApp and HydraHeadApps, we can do whatever we want!** 🤪')

    #run with default menu layout
    #app.run()

    #if the menu is looking shit, use some sections
    app.run(complex_nav)
```