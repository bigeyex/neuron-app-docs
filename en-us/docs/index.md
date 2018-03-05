# The Neuron Language

**Makeblock Neuron** is a visual programming tool for making with electronics. It uses a flow-based model: users create logic between nodes by connecting inputs with outputs. Its purpose is to make electronic building easy and fun.

Why is Neuron built since there are other visual programming languages (such as Scratch)? First, Neuron is built for creativity with electronics from the ground up. Design choices are made for how the maker interacts with the electronics, not the procedure of a single actor (such as a computer or a robot). There are several design values we took when we are designing Neuron:

- **Descriptive**: we want people to think more about what they are going to make, not how the code works. This results in a mind map like flow-based interface. "Descriptive" also means as stateless as possible: forcing the user to think about the state of the system and the position of the "execution cursor" shifts the user's focus away from what they are creating and how the creation interacts with the people and the world.
- **Serendipity**: we want people to experiment different possibilities of their creation. For example, the value of a sensor or a period of rhythm could make some sound, but the same output can also be used in making colorful lights. Creativity comes from combining various affordances of the physical world.
- **Visibility**: by default, users can see the status of every output. This provides a quick debugging tool that whenever there is something goes wrong, the user could check the result of every intermediary step (which is one of the essences of debugging). Besides, exposing the internal logic to the user encourages a deeper understanding of how math and logic work with electronics creation. 
- **Focus on data-flow**: we believe that understanding data processing is a key skill for success in this digital world. Thus, everything happens in Neuron is a constant update of data values. Events are expressed by flipping the value from "false" to "true"(which is often called a "rising edge" in electronic engineering). There are no concepts of "triggering" internally, and data is the blood that flows in a Neuron program. 
- **Ease of use and full of expressiveness**: Neuron is a balance between providing a powerful tool and a tool that is easy to get hands on. We are not sticking to a single programming paradigm, and we are always ready to make compromises, but we are also aware of the cost of breaking the consistency. We try to make it as simple as possible, but we also do not want to limit the possibility of what users can make. People will not find themselves empowered if they cannot make anything they wish. 
- **Context-aware**: since Neuron is created for electronics creation, it could simplify its UI by providing options just when using the relative functions. For example, the "color" node will be available when there are LEDs or other entities that use or express colors. In this way, the UI could be less cluttered and easier to use by common folks. And the experts will not find its inefficient too.
- **Gradually enhancing**: we do not want to introduce too many concepts at first sight. So many nodes can react to both simple and complex inputs. For example, if you link a button to an RGB LED, the LED will simply light up when the button is pressed; but if you link the Button to a Color, then to an RGB LED, then the LED will light up in the exact color you told it. We hope Neuron will follow the user’s anticipation at most of the time, so the user does not need to take time working on detailed concepts such as data types.

Is Neuron an object-oriented language? Maybe not. Each node is an independent computation unit, and it exchanges data through public interfaces. But nodes usually do not have states except some rare cases, and Neuron does not have inheritance and polymorphism, which are main features of an object-oriented language.

Is Neuron functional? Mostly. Neuron tries to be stateless, and the nodes are like functions in a Lisp-like language. In fact, Neuron is to a large extent inspired by functional languages. But is some cases, such as when expressing time series, nodes, can update data by itself (having states). This breaks the law of being functional but is necessary to play with electronics. After all, we do not want every user to wrap his/her head around thinking functional.

# Some important concepts about Neuron

## Nodes

**Nodes** are basic units of Neuron programs. Each node receives data from its zero or more **inputs**, and put its calculation results to zero or more **outputs**. 

Nodes that represent an electronic module is called “**hardware nodes**.” They appear at the top of the screen when a Neuron electronic block is connected. Nodes that generate and process data are called “**logic nodes**.” Such nodes include calculations such as ADD or SUBSTRACT, logic operations such as AND or OR, and time-related operations such as DELAY or TIMER. 

Tapping on some nodes, such as **Number** or **Gyro Sensor**, will bring up a config panel. Settings in the panel will change the behavior of the node. Tapping on other nodes, such as LED PANEL, will bring up a list of **Companion Nodes**. You may then place them on the canvas. Companion Nodes represents important data related to the original node. For example, companion node Color provides COLOR for the RGB LED, while companion node IMAGE can set the image displayed on the LED PANEL.

## Data types

Neuron supports the following data types:

- **Yes/No** (Boolean): outputs of this type will have a "Y/N" mark on its output. 
- **Numbers**: the output will display a number
- **Strings**: a string of text
- **Objects**: such as colors, or color stripe patterns.
- Errors: some errors happened when computing the node. This is usually because the inputs or the config is incorrect

Some nodes receive more than one type of data. For example, the DC Motor Node will let the motors run when receives “yes” (the DC Motor has 2 motor ports, and a “yes” will let both motors run, so no matter which port you connected your motor on, it will run); it will let the motors run at a certain speed if provided a number; and it will set individual speeds for each motor if received data from its companion node named **Motor Speed**. 

But what if the node is provided with incompatible data? Nearly every node in Neuron accepts yes/no data and have a default action upon that. For lights, it is turning on; for buzzers, it is making a beeping sound. If a piece of incompatible data is provided, the node will convert it to yes/no data. The rule is: **the number zero, errors and “no” itself will be converted to “no,” and anything else is converted to “yes.”** In the following part of this manual, I will use “**the input is considered ‘yes’**” when I refer to an input other than “no,” errors or “zero.”

## Mixing Inputs

If there are more than 2 outputs connected to a single input, the node will mix all the sources before it processes the input. The following rules apply:

1.	If there is any input that is not “no”(false), the result will not be “no” (which also means, an “or” logic is placed before every input).
2.	If there are more than one inputs that are not “no,” the most recently updated one count. For example. If you tell the light to turn red 2 seconds before, then you tell it to turn green, it will stay green. If you tell the light to be red and green at the same time, the result could be red or green (randomly), because the light cannot be red and green at the same time. 
 
## Hanging Inputs

When a node has an input that is not linked to any of the outputs, it is called a hanging input. Many of Neuron nodes, such as NUMBER, COLOR, or INTERVAL, have the following behavior: when the input is hanging, it will act as if the input is “yes.” This turns the Number and Color as a constant, and the Interval as an interval timer. When an actual input is provided, these nodes will be controlled by the input: it will only output its usual value when the input is “yes,” otherwise the node will output “no.”  

