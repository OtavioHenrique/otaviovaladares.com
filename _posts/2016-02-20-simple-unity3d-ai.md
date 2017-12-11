---
layout:     post
title:      "Simple Enemy AI System for Unity3D"
sybtitle:   "Simple tutorial to get start with enemies"
date:       2016-02-30 00:00:00
author:     "Otavio Henrique"
header-img: "img/post-simple-unity3d-ai/ai.jpg"
catalog:    true
tags:
    - Unity3d
    - AI
---

In this post I am going to explain how I have developed a simple AI system for
the enemies of my final project on technical college. The idea is a simple
script that made the enemy’s walks randomly on the on the scene, chase and
attack the player.

The IA script that I developed is based on several games, especially in stealth
games. The enemy will select a random destination when the game is started and
will walking in this direction, when he comes close to the selected random
destination, and another random destination is selected and it starts all the
process again. If while he is walking toward a point he sees the player, he
starts chasing the player and if he reaches the player, he is going to attack
attack him.

For this example I will use Unity 5 game engine but you can easily make changes
and use in other engines like Unreal.

I will not explain basics features of Unity in this post or programming logic,
If you have any question please read the reference links or ask me here or on my
Twitter.

## First Step: Build Scene

![](https://cdn-images-1.medium.com/max/800/0*EL4ftSsWXOJgTGFL.png)

For this example I have build a simple maze using 3D blocks and positioned the
enemy(blue sphere) on the right top corner and our player on lower left corner
(red sphere).<br> Our player its a simple FPSController (You can find this
script on Unity Standard Assets), read more
[here](https://docs.unity3d.com/Manual/class-CharacterController.html).

## Second Step: Create Navigation Mesh

For our enemy walk on the scene the easiest method is create a navigation mesh
on the scene and add a Nav Mesh Agent on the components of our enemy (3D
sphere).

*“The navigation system allows you to create characters that can intelligently
move around the game world, using navigation meshes that are created
automatically from your Scene geometry.“*

![](https://cdn-images-1.medium.com/max/800/0*dHTf-6OyBjSKrluP.png)

*Pay atention on your navigation mesh, it must be bypassing all walls, if not
your enemy will not work correctly.*

This simple step will make the next things easy.

## Third Step: Our script

To start we need to declare and define our random destination using Random.range
function and get navMeshAgent component. After this our code looks like this:

```cs
public int currentRandomPoint;
private NavMeshAgent navMesh;

void Start () {
  currentRandomPoint = Random.Range(0, randomPoints.Length);
  navMesh = transform.GetComponent<NavMeshAgent>();
}
```

If you will use annimator component to use animations on the enemy you should
declare and use on the void start too, like this:

```cs
protected Animator animator;

animator = GetComponent<Animator>();
```

On void update the first thing we need to do is calculate player distance and
the random point distance, for this we use Vector3.

```cs
playerDist = Vector3.Distance(Player.transform.position, transform.position);
irandomPointDist = Vector3.Distance(randomPoints[currentRandomPoint].
transform.position, transform.position)
```

And declare a RaycastHit, this raycasthit will will ensure that our enemy does
not see the player behind walls.

```cs
RaycastHit hit;
```

You can read about *Physics.Raycast*
[here](https://docs.unity3d.com/ScriptReference/Physics.Raycast.html).

```cs
if (Physics.Raycast (transform.position, direction, out hit, 1000) &&
playerDist < perceptionDistance ) {
  if (hit.collider.gameObject.CompareTag("Player")) {
    seeingPlayer = true;
  } else {
    seeingPlayer = false;
  }
}
```

And walk while our player distance is greater than perception distance.

```cs
if ( playerDist > perceptionDistance)
  walk();
```

And our walk method is basically set navMesh acceleration, speed and
destination, like this:

```cs
void walk () {
  if (chasing == false) {
    navMesh.acceleration = 1;
    navMesh.speed = walkVelocity;
    navMesh.destination = randomPoints[currentRandomPoint].position;
  } else if (chasing == true) {
    chaseTime = true;
  }
}
```

And now with RaycastHit, player distance and random point distance our script is
making something like this:

![](https://cdn-images-1.medium.com/max/800/0*ifSApAWKUH3wDLuq.png)

The orange dotted thick line it’s our script calculating the player distance,
the blue dotted line it’s the random point distance and the green arrow is our
RaycastHit.

And here the other 3 behaviors of our enemy:

```cs
void look () {
  navMesh.speed = 0;
  transform.LookAt (Player);
}

void chase () {
  navMesh.acceleration = 5;
  navMesh.speed = chaseVelocity;
  navMesh.destination = Player.position;
}

void attack () {
  navMesh.acceleration = 0;
  navMesh.speed = 0;
  attacking = true;
}
```

If you are working with animator and animations you can call your animator
variables here, like this example:

```cs
animator.SetFloat("Speed", 0.5f);
```

The next steps is make a lot of checkings to attack, chase, stop chasing and all
that stuffs, I will not going into details, but if you have doubts you can
contact me as I told above.

Here’s the next checkings:

```cs
if ( playerDist <= perceptionDistance && playerDist > chaseDistance) {
  if (seeingPlayer == true)
    look();
  else
    walk();
}

if ( playerDist <= chaseDistance && playerDist > attackDistance) {
  if (seeingPlayer == true) {
    chase();
    chasing = true;
  } else {
    walk();
  }
}

if (playerDist <= attackDistance && seeingPlayer == true)
  attack();

if (randomPointDist <= 8) {
  currentRandomPoint = Random.Range(0, randomPoints.Length);
  walk();
}

if (chaseTime == true)
  chaseStopwatch += Time.deltaTime;

if (chaseStopwatch >= 5 && seeingPlayer == false) {
  chaseTime = false;
  chaseStopwatch = 0;
  chasing = false;
}

if (attacking == true)
  attackingStopwatch += Time.deltaTime;
```

And the attack checking:

```cs
 if (attackingStopwatch >= attackTime && playerDist <= attackDistance) {
   attacking = true;
   attackingStopwatch = 0;
 }

 if (attackingStopwatch >= attackTime && playerDist > attackDistance) {
   attacking = false;
   attackingStopwatch = 0;
 }
 ```

If you want to cause damage on player you should add something like this:

```cs
playerLife = playerLife - enemyDamage;

if (playerLife <= 5) {
  Application.LoadLevel(GameOverSceneName);
} else if (attackingStopwatch >= attackTime && playerDist > attackDistance) {
  attacking = false;
  attackingStopwatch = 0;
}
```

I really recommend your playerLife be a variable of an script of the player.

Finally, if you want that’s your player can attack the enemy you can add
rigidBody component to your enemy and make this:

```cs
void OnCollisionEnter (Collision col) {

  if(col.transform.tag == "Bullet"){
    enemyLife -= 10;
    if (enemyLife <= 0) {
      Destroy (gameObject);
    }
  }
}
```

And now we finished our simple artificial intelligence script and you can use
for your simple game, study and improve, in a few months I will post the second
part explaining how to improve this script, adding field of vision, sounds and
etc..

You can view the full script/project on my github, and sorry for the quality of
the project/code, I was 16 years old when I wrote this.

[https://github.com/OtavioHenrique/simpleAI](https://github.com/OtavioHenrique/simpleAI)

And ask me on my Twitter or Linkedin

**Thanks guys, see you soon.**
