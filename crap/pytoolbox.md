---
author: Ethan Wilkes
---
# This Week's CRAP
Welcome to this week's CRAP meeting!

Today's Topic: **PyToolbox**

---

# What is PyToolbox

- Implementation of C++ Toolbox in Python
- Currently focusing on CDX Mesh

---

# Why CDX Mesh
What we want to avoid

```
~~~graph-easy --as=boxart
[ Messenger 1 ] -> [ Messenger 2 ], [ Messenger 3 ], [ Messenger 4 ]
[ Messenger 2 ] -> [ Messenger 1 ], [Messenger 4]
[ Messenger 3 ] -> [ Mesenger 1 ], [ Messenger 2 ], [ Messenger 4 ]
~~~
```

- No separation from publishers and subscribers
- Many messengers all connected
- Connections done by ports, hard to remember what is taken and what is open
- Long thought process for each connection

---

# Why CDX Mesh

```
~~~graph-easy --as=boxart
[ Publisher ] - topic 1 -> [ B ]
[ B ] - topic 1 -> [ Subscriber 1 ], [ Subscriber 2 ]
[ B ] .. topic 2 ..> [Subscriber 3 ]
~~~
```

- From a user perspective, a publisher simply connects to the broker and publishes messages on a topic
- From a user perspective, a subscriber simply connects to the broker and receives messages on a topic
- Simple thought process for any new connections
- Topics are strings, so they are easy to remember since their purpose can be encoded in their name
- Can support multiple transport and messaging protocols with just the switch of a config flag

---

# Why Python

Most poular language on GitHub (by far):

- Most stars
- Most pull requests
- Most pushes
- Most issues

Other stats:

- Most popular language on the TIOBE Index
- Fourth most popular on Stack Overflow Developer survey for professional developers, behind JS, HTML, SQL

---

# Why Python

- Fast development
- Easy to maintain
- Easy to review
- TONS of libraries
- TONS of community support


Good for anything that does not have a very strict performance requirement.

---

# PyToolbox

Simple pub / sub creation

```python    
# Create connection to the broker
config = load_cdx_config(os.path.join(script_dir, "config"), "cdxbroker.toml")
cdx = CdxConnection(config, "Subscriber")
# Create pub/sub
sub = await cdx.spawn_subscriber("crap.topic")
pub = await cdx.spawn_publisher("crap.topic")
# Send a test message
await pub.put(json.dumps({message: "Hello CRAP!"}))
msg = await sub.get()
```

Additionally, the PyToolbox supports the current broker, and the new (blazingly fast) Rust broker

---

# Stats

## Current Usage

PyToolbox used in the creation of services for the MENTOR demo:

- Audio Manager
- NLP Service
- TTS Service

## Results

Brett had working prototypes for these three services in 5 working days (Nov 16 - Nov 22)

These services:

- Interact with each other over CDX Mesh
- Send voice from microphones
- Play voice on speakers
- Train a classification model
- Use a classification model to match text phrases
- Speak matched phrases over speakers

These are complicated services, leveraging the Python ecosystem and CDX Mesh.
