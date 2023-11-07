---
description: Adding Rive to your Bevy games
---

# Getting Started

## Example Project

To quickly experiment with rive-bevy, run one of our example projects.

```bash
git clone https://github.com/rive-app/rive-bevy
cd rive-bevy/
cargo run --example ui-on-cube
```

There are a number of demos/games in the [examples](https://github.com/rive-app/rive-bevy/tree/main/examples) folder that showcase various Rive features.

## Installation

Add the [Rive Bevy GitHub repository](https://github.com/rive-app/rive-bevy) as a dependency to your `Cargo.toml` file:

```toml
rive-bevy = { git = "https://github.com/rive-app/rive-bevy" }
```

Import dependencies:

```rust
use rive_bevy::{StateMachine, RivePlugin, SceneTarget, SpriteEntity};
```

Register the Rive plugin:

```rust
fn main() {
    App::new()
        .add_plugins(DefaultPlugins)
        .add_plugins(RivePlugin) // Add Rive
        .add_systems(Startup, setup_animation)
        .run()
}

```

## Assets

All Rive assets (_`.riv`_ files) should be added to the `assets` folder, and can be loaded with Bevy's [AssetServer](https://docs.rs/bevy/latest/bevy/asset/struct.AssetServer.html):

```rust
asset_server.load("your_file.riv")  
```

## Loading Animations

You can choose to load a [LinearAnimation](https://help.rive.app/runtimes/playback) or [StateMachine](https://help.rive.app/runtimes/state-machines) from the provided Rive asset.

**LinearAnimation**:

```rust
let linear_animation = LinearAnimation {
    riv: asset_server.load("marty.riv"),
    // Optionally provide animation name to load
    handle: rive_bevy::Handle::Name("Animation1".into()),
    // Optionally provide artboard name to load
    artboard_handle: rive_bevy::Handle::Name("New Artboard".into()),
    ..default()
};
```

**StateMachine:**

```rust
let state_machine = StateMachine {
    riv: asset_server.load("marty.riv"),
    // Optionally provide State Machine name to load
    handle: rive_bevy::Handle::Name("State Machine 1".into()),
    // Optionally provide artboard name to load
    artboard_handle: rive_bevy::Handle::Name("New Artboard".into()),
    ..default()
};
```

## Rendering

Rive-Bevy renders to an [Image Texture](https://docs.rs/bevy/latest/bevy/render/texture/struct.Image.html) that you can display in your scene by attaching it to a [SpriteBundle](https://docs.rs/bevy/latest/bevy/sprite/struct.SpriteBundle.html) for 2D or a [StandardMaterial](https://docs.rs/bevy/latest/bevy/prelude/struct.StandardMaterial.html) for 3D.

#### 2D Scene / UI

The following example demonstrates drawing a `StateMachine` to a `SpriteBundle` in a 2D scene and enables user interaction via a mouse to drive Rive Listeners.

```rust
use bevy::{prelude::*, render::render_resource::Extent3d, window};
use rive_bevy::{RivePlugin, SceneTarget, SpriteEntity, StateMachine};

fn main() {
    App::new()
        .add_plugins(DefaultPlugins)
        .add_plugins(RivePlugin)
        .add_systems(Startup, setup_animation)
        .add_systems(Update, window::close_on_esc)
        .run()
}

fn setup_animation(
    mut commands: Commands,
    mut images: ResMut<Assets<Image>>,
    asset_server: Res<AssetServer>,
) {
    let mut animation_image = Image::default();

    // We fill the CPU image with 0s before sending it to the GPU.
    animation_image.resize(Extent3d {
        width: 512,
        height: 512,
        ..default()
    });

    let animation_image_handle = images.add(animation_image.clone());

    commands.spawn(Camera2dBundle { ..default() });

    let sprite_entity = commands
        .spawn(SpriteBundle {
            texture: animation_image_handle.clone(),
            transform: Transform::from_scale(Vec3::splat(1.0)),
            ..default()
        })
        .id();

    let state_machine = StateMachine {
        riv: asset_server.load("rating-animation.riv"),
        // Optionally provide state machine name to load
        handle: rive_bevy::Handle::Name("State Machine 1".into()),
        // Optionally provide artboard name to load
        artboard_handle: rive_bevy::Handle::Name("New Artboard".into()),
        ..default()
    };

    commands.spawn(state_machine).insert(SceneTarget {
        image: animation_image_handle,
        // Adding the sprite here enables mouse input being passed to the Scene.
        sprite: SpriteEntity {
            entity: Some(sprite_entity),
        },
        ..default()
    });
}

```

1. Create a new `Image` and `resize` it to be the same aspect ratio as your Rive artboard. Note that an Artboard my have clipping disabled and elements could be drawn outside of the bounds. In that case you'd want to size the Image to take up the drawable area.
2. Spawn a new `SpriteBundle` providing the image as a texture.
3. Create a `StateMachine`, optionally provide the `handle` (state machine name) and `artboard_handle` (artboard name).
4. Spawn a new `SceneTarget`, optionally provide the `sprite` argument which enables mouse inputs to pass to the scene. This is only relevant for **State Machine** animations as **Linear Animations** cannot interact with Rive listeners.

#### 3D Scene

The following demonstrates drawing a `StateMachine` to a `StandardMaterial` in a 3D scene and enables user interaction via a mouse to drive Rive Listeners.

```rust
use bevy::{prelude::*, render::render_resource::Extent3d, window};
use rive_bevy::{RivePlugin, SceneTarget, StateMachine};

fn main() {
    App::new()
        .add_plugins(DefaultPlugins)
        .add_plugins(RivePlugin)
        .add_systems(Startup, setup_animation)
        .add_systems(Update, rotate_cube)
        .add_systems(Update, window::close_on_esc)
        .run()
}

#[derive(Component)]
struct DefaultCube;

fn setup_animation(
    mut commands: Commands,
    mut images: ResMut<Assets<Image>>,
    mut meshes: ResMut<Assets<Mesh>>,
    mut materials: ResMut<Assets<StandardMaterial>>,
    asset_server: Res<AssetServer>,
) {
    let mut animation_image = Image::default();

    // We fill the CPU image with 0s before sending it to the GPU.
    animation_image.resize(Extent3d {
        width: 512,
        height: 512,
        ..default()
    });

    let animation_image_handle = images.add(animation_image.clone());

    let cube_size = 4.0;
    let cube_handle = meshes.add(Mesh::from(shape::Box::new(cube_size, cube_size, cube_size)));

    let material_handle = materials.add(StandardMaterial {
        base_color_texture: Some(animation_image_handle.clone()),
        reflectance: 0.02,
        unlit: false,
        ..default()
    });

    let cube_entity = commands
        .spawn((
            PbrBundle {
                mesh: cube_handle,
                material: material_handle,
                transform: Transform::from_xyz(0.0, 0.0, 1.5),
                ..default()
            },
            DefaultCube,
        ))
        .id();

    commands.spawn(PointLightBundle {
        point_light: PointLight {
            intensity: 3000.0,
            ..default()
        },
        // Light in front of the 3D camera.
        transform: Transform::from_translation(Vec3::new(0.0, 0.0, 10.0)),
        ..default()
    });

    commands.spawn(Camera3dBundle {
        transform: Transform::from_xyz(0.0, 0.0, 15.0).looking_at(Vec3::ZERO, Vec3::Y),
        ..default()
    });

    let linear_animation = StateMachine {
        riv: asset_server.load("rating-animation.riv"),
        // Optionally provide state machine name to load
        handle: rive_bevy::Handle::Name("State Machine 1".into()),
        // Optionally provide artboard name to load
        artboard_handle: rive_bevy::Handle::Name("New Artboard".into()),
        ..default()
    };

    commands.spawn(linear_animation).insert(SceneTarget {
        image: animation_image_handle,
        // Adding the sprite here enables mouse input being passed to the Scene.
        mesh: rive_bevy::MeshEntity {
            entity: Some(cube_entity),
        },
        ..default()
    });
}

fn rotate_cube(time: Res<Time>, mut query: Query<&mut Transform, With<DefaultCube>>) {
    for mut transform in &mut query {
        transform.rotate_x(0.6 * time.delta_seconds());
        transform.rotate_y(-0.2 * time.delta_seconds());
    }
}

```

### Additional Resources

* [Simple 2D example](https://github.com/rive-app/rive-bevy/blob/main/examples/simple\_2d.rs)
* [Simple 3D example](https://github.com/rive-app/rive-bevy/blob/main/examples/simple\_3d.rs)
