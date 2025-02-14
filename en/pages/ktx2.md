# Using KTX2

On Safari iOS your page may refresh several times then give up because of memory constraint because you're using too many 4k or 8k jpg/png textures. The GPI memory constraint applies to all images that are uploaded to the GPU, with 3d models, or aframe `material` component that references an image.

## Install toktx and gltf-transform commands

Install the `toktx` command from https://github.com/KhronosGroup/KTX-Software

On Ubuntu with bash shell:

```sh
sudo apt install git git-lfs build-essential cmake
git clone git@github.com:KhronosGroup/KTX-Software.git
cd KTX-Software/
./install-gitconfig.sh
./ci_scripts/smudge_date.sh
cmake . -B build
cmake --build build
export PATH="/home/youruser/KTX-Software/build/tools/toktx:$PATH"
# modify youruser and full path accordingly, you can put this export line in your ~/.bashrc
```

Install the `gltf-transform` command from npm https://www.npmjs.com/package/@gltf-transform/cli

```sh
npm install --global @gltf-transform/cli
```

## Optimizing glTF models

You can use the `gltf-transform optimize` command to optimize glTF models, for textures by default it auto recompresses in original format and resize to 2048 max.
You can add `--texture-compress ktx2` option to convert the textures to ktx2 format (etc1s / uastc) saving a lot of GPU memory, but it won't resize to 2048 max anymore, you will need to run a `gltf-transform resize` first to resize the jpg/png textures to 2048 max.
Be aware it can take a lot of time to convert the textures to ktx2 format that is using the toktx command, depending of the model it could take 20 or 40 min... probably because of the default values used by `gltf-transform` for the `toktx` command that uses `--uastc 4` (level 4 is very slow but higher quality).
The command also applies other optimizing steps, like draco compression.

An example of usage:

```sh
gltf-transform resize --width 2048 --height 2048 car.glb car_resized.glb
gltf-transform optimize --texture-compress ktx2 car_resized.glb car_optimized_ktx2.glb
```

It optimizes the `car.glb` model generating a new `car_optimized_ktx2.glb` file.
If the mesh has too much degradation, you may want to disable the `simplify` step (Simplify mesh geometry with meshoptimizer) with the `--simplify false` option.
See options in [gltf-transform cli documentation](https://gltf-transform.dev/cli) and the default steps with the default values in [cli.ts](https://github.com/donmccurdy/glTF-Transform/blob/main/packages/cli/src/cli.ts#L235) (search for the OPTIMIZE comment in this file).
Another step you may want to disable is the palette step "Creates palette textures and merges materials" with `--palette false` if you use the model with a color picker in your scene.

For use the new model with aframe, you will need to have the draco decoder path and basis transcoder path correctly configured like this:

```js
<a-scene gltf-model="dracoDecoderPath:https://www.gstatic.com/draco/versioned/decoders/1.5.6/;meshoptDecoderPath:https://unpkg.com/meshoptimizer@0.19.0/meshopt_decoder.js;basisTranscoderPath:https://cdn.jsdelivr.net/npm/three@0.154.0/examples/jsm/libs/basis/">
```

Check the [gltf-model aframe documentation](https://github.com/aframevr/aframe/blob/master/docs/components/gltf-model.md#using-compression) for up to date versions to use here.

You can look at the memory usage of your model with https://gltf.report/ site.
Vincent: The memory reduction is insane, for a scene that I couldn't load on Quest 1, it was vram 1.1 GB, with optimization it shrinked to vram 220 MB.

## Using meshopt instead of draco

Some models like avatars from [VALID](https://github.com/xrtlab/Validated-Avatar-Library-for-Inclusion-and-Diversity---VALID) may have a better size reduction if you compress the geometries with meshopt instead of draco which is the default. Here is an example of using meshopt compression and webp textures with simplify step disabled:

```sh
gltf-transform optimize --simplify false model.glb model_webp_meshopt.glb --texture-compress webp --compress meshopt
```

# Converting png to ktx2

Vincent: Also I did recently an experience with 8192x8192 360 stereo image generated with AI models, that I converted to ktx2 with the `basisu` command and loaded with `KTX2Loader`. The images loads smoothly on Quest 1.

Maybe some of you remember the article [Using Basis Textures in Three.js](https://medium.com/samsung-internet-dev/using-basis-textures-in-three-js-6eb7e104447d) written by Ada.
It was with the `.basis` extension, now the standard is `.ktx2`.

To install the `basisu` command on Ubuntu:

```sh
sudo apt install git build-essential cmake
git clone https://github.com/BinomialLLC/basis_universal.git
cd basis_universal
cmake CMakeLists.txt
make
```

To convert an image:

```qh
./bin/basisu -y_flip -ktx2 image.png
```

The command creates an `image.ktx2` file that you can preview with https://sandbox.babylonjs.com/

To load the image in aframe, that's similar to the Ada's article, but with `ktxLoader`:

```js
const ktxLoader = AFRAME.scenes[0].systems['gltf-model'].getKTX2Loader();
ktxLoader.load(
  '/image.ktx2',
  (texture) => {
    texture.encoding = THREE.sRGBEncoding;
    material.map = texture;
    material.needsUpdate = true;
  },
  undefined,
  (error) => {
    console.error(error);
  }
);
```
