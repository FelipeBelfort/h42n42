# H42N42

> Simulate the life of a population of creatures threatened by a terrifying virus.

**Status: work in progress.** Sections marked `TODO` will be completed as the project advances.

## Description

H42N42 is an interactive browser game written entirely in [OCaml](https://ocaml.org) with the [Ocsigen](https://ocsigen.org) framework. Small creatures called **Creets** live in a land bordered by a river polluted by the H42N42 virus. The player keeps the population alive as long as possible by keeping Creets away from the river and carrying the sick ones to the hospital.

### Objectives

This project is an introduction to client-side web programming in OCaml:

- Run an OCaml program in the browser with the **Js_of_ocaml** compiler
- Generate statically validated HTML with **TyXML** (through **Eliom**)
- Manipulate the DOM and call JavaScript methods from OCaml
- Handle mouse events with `Lwt_js_events`
- Program cooperative threads in monadic style with **Lwt** (one thread per Creet)

## Prerequisites

- [Docker](https://docs.docker.com/get-docker/)
- [Docker Compose](https://docs.docker.com/compose/install/)

Nothing else is needed on the host: OCaml, Eliom and Js_of_ocaml are installed inside the container.

| Component | Version |
|-----------|---------|
| OCaml | TODO (>= 4.14) |
| Eliom | TODO (>= 10.0) |
| Js_of_ocaml | TODO (>= 5.0) |

## Installation

```sh
git clone https://github.com/FelipeBelfort/h42n42.git h42n42
cd h42n42
```

## Running the application

```sh
docker-compose up --build
```

The first build compiles the whole OCaml toolchain and can take several minutes.

To stop the application:

```sh
docker-compose down
```

### Makefile shortcuts

The Makefile wraps the Docker commands so they do not have to be typed in full:

| Command | Equivalent |
|---------|------------|
| `make up` | `docker-compose up --build` |
| `make down` | `docker-compose down` |
| `make logs` | `docker-compose logs -f` |

## Accessing the application

Open <http://localhost:8080> in a web browser.

## Building without Docker

The compilation rules are the ones the Dockerfile uses inside the container. They can also be run on the host with a local opam switch containing Eliom:

| Command | Effect |
|---------|--------|
| `make` / `make all` | Build the application |
| `make clean` | Remove intermediate build files |
| `make fclean` | Remove everything that was generated |
| `make re` | `fclean` then `all` |

TODO: command to start the local test server.

## Game rules

**Goal:** keep at least one healthy Creet alive for as long as possible.

### The world

- The **toxic river** runs along the top of the play area.
- The **hospital** sits at the bottom.
- Creets move in straight lines, sometimes change direction, and bounce off the edges.
- As long as one healthy Creet exists, new Creets are born regularly.
- All Creets speed up over time.

### Contamination

- A Creet that touches the river becomes **sick**: it changes colour and moves 15 % slower.
- A sick Creet touching a healthy one has a 2 % chance per iteration to infect it.
- A newly sick Creet may turn into a special type, permanently:

| Type | Chance | Behaviour | Death |
|------|--------|-----------|-------|
| **Berserk** | 10 % | Diameter grows by 10 % every 10 seconds | At 4 times its original diameter |
| **Mean** | 10 % | 15 % smaller, chases the nearest healthy Creet | After 1 minute |

### What the player can do

- **Drag and drop** a Creet anywhere with the mouse. A Creet being dragged cannot be infected.
- **Heal** a sick Creet by dropping it on the hospital. A sick Creet that wanders into the hospital by itself is not healed.
- Berserk and mean Creets cannot be grabbed or healed.

### Colours

| State | Colour |
|-------|--------|
| Healthy | TODO |
| Sick | TODO |
| Berserk | TODO |
| Mean | TODO |

### Game over

The game ends when no healthy Creet remains. A "GAME OVER" message is displayed.

## Project structure

```
.
docker-compose.yml
README.md
src/               # .eliom source files
Eliom, Js_of_ocaml, Lwt, TyXML documentation
- [Writing a client/server Eliom application](https://ocsigen.org/tuto/latest/application.html)
- [OCaml documentation](https://ocaml.org/docs)

