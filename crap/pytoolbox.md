---
author: Ethan Wilkes
---
# This Week's CRAP
Welcome to this week's CRAP meeting!

Today's Topic: **PyToolbox**

---

# A Word from our Sponsors

Announcements from management

---

# What is PyToolbox

- Implementation of C++ Toolbox in Python
- A library for commonly used functionality in our proprietary software

## Current Work

- ✅ Clients for connecting to CDX Mesh
- Next, working on a service architecture

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

- `Decoupling` - services need to know the details of how to communicate with each other
- `Maintainability` - a change to one service can greatly affect the logic in other services
- `Ports` - Using ports can get quite confusing. You need to make sure no ports in your service are already 
being used for other communications

---

# Why CDX Mesh

```
~~~graph-easy --as=boxart
[ Publisher ] - topic 1 -> [ B ]
[ B ] - topic 1 -> [ Subscriber 1 ], [ Subscriber 2 ]
[ B ] .. topic 2 ..> [Subscriber 3 ]
~~~
```

- `Decoupling` - services send and receive on *topics* from the broker. They do not know anything about other services
- `Maintainability` - since services do not need to know about each other, changes in one need not affect the others
and it is trivial to add new pubs / subs
- `Topics` - topics with meaning are used instead of ports, and can be reused by anyone who has something to say on that topic
(ex. "scenario.cmd.priamry")

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

- Interact with the C++ services within our MENTOR system
- Interact with each other
- Send voice from microphones
- Play voice on speakers
- Use a nlp classification model to match text phrases
- Send matched phrases as commands throughout the system

These are complicated services, leveraging the Python ecosystem and PyToolbox.
