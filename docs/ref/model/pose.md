---
sidebarDepth: 2
---
# 🤸‍♀️ Pose


<div class="row align-top">
  <div class="col-6">
    <p><img alt="A person striking various poses, with a wireframe overlaid on top of them" src="https://media0.giphy.com/media/VJ7aDV6F5id8wY2Ff0/giphy.gif" /></p>
    <ul>
      <li>Powered by <a href="https://www.npmjs.com/package/@mediapipe/pose">MediaPipe's Pose</a></li>
      <li>Full <a href="https://google.github.io/mediapipe/solutions/pose.html">technical documentation</a></li>
    </ul>
  </div>
  <div class="col-6">
    <Window title="Overview and basic demo">
      <section>
        <ul>
          <li>🤸‍♀️33 3D pose landmarks</li>
          <li>🤺 Optional body segmentation mask</li>
          <li>📅 Extra helpers and plugins coming soon</li>
        </ul>
        <p>This model doesn't come with any bonuses or plugins yet but they'll come soon. The API will remain exactly the same, so feel free to started with this model today!</p>
        <div>
          <HandsfreeToggle class="full-width handsfree-hide-when-started-without-pose" text-off="Try basic Pose demo" text-on="Stop Pose Model" :opts="demoOpts" />
          <button class="handsfree-show-when-started-without-pose handsfree-show-when-loading" disabled><Fa-Spinner spin /> Loading...</button>
          <button class="handsfree-show-when-started-without-pose handsfree-hide-when-loading" @click="startDemo"><Fa-Video /> Try basic Pose demo</button>
        </div>
      </section>
    </Window>
  </div>
</div>

## Usage

### With defaults

```js
const handsfree = new Handsfree({pose: true})
handsfree.start()
```

### With config

```js
const handsfree = new Handsfree({
  pose: {
    enabled: false,

    // [0-2] Higher values are more accurate but slower
    modelComplexity: 1,
    
    // Helps reduce jitter over multiple frames if true
    smoothLandmarks: true,

    // If true it also generates a segmentation map
    enableSegmentation: false,
    // Helps reduce jitter over multiple frames if true
    smoothSegmentation: true,

    // Minimum confidence [0 - 1] for a person detection to be considered detected
    minDetectionConfidence: 0.5,

    // Minimum confidence [0 - 1] for the pose tracker to be considered detected
    // Higher values are more robust at the expense of higher latency
    minTrackingConfidence: 0.5
  }
})

handsfree.start()
```

## Data

### Pose Landmarks
![](https://google.github.io/mediapipe/images/mobile/pose_tracking_full_body_landmarks.png)
<br><small>Image source, MediaPipe: [https://google.github.io/mediapipe/solutions/pose#pose-landmark-model-blazepose-tracker](https://google.github.io/mediapipe/solutions/pose#pose-landmark-model-blazepose-tracker)</small>

#### `.poseLandmarks` (normalized 3D points)

You can access the landmarks with:

```js
// An array of landmark points for the face
handsfree.data.pose.poseLandmarks == [
  // Landmark 0
  {x, y, z, visibility},
  // Landmark 1
  {x, y, z, visibility},
  // ...
  // Landmark 32
  {x, y, z, visibility}
]

// Nose
handsfree.data.pose.poseLandmarks[0].x
handsfree.data.pose.poseLandmarks[0].y
handsfree.data.pose.poseLandmarks[0].z
// The confidence in this pose landmark
handsfree.data.pose.poseLandmarks[0].visibility
```

Each of these 33 landmarks are normalized to the video, where `0` represents top/left and `1` is bottom/right for `.x` and `.y`. The `.z` is normalized with the a similar magnitude as `.x`, and the depth is relative to the hips midpoint. `.visibility` is a range between `[0, 1]` that describes how likely that point is visible `[1]` or not `[0]`

#### `.poseWorldLandmarks`

This is like `.poseLandmarks` but where the points are measured in meters relative to the midpoint of the hips.

```js
// An array of landmark points for the face
handsfree.data.pose.poseWorldLandmarks == [
  // Landmark 0
  {x, y, z, visibility},
  // Landmark 1
  {x, y, z, visibility},
  // ...
  // Landmark 32
  {x, y, z, visibility}
]

// Nose
handsfree.data.pose.poseWorldLandmarks[0].x
handsfree.data.pose.poseWorldLandmarks[0].y
handsfree.data.pose.poseWorldLandmarks[0].z
// The confidence in this pose landmark
handsfree.data.pose.poseWorldLandmarks[0].visibility
```

### `.segmentationMask`

In addition to the landmarks you can also extract the pixels that make up the detected person, this is called segmentation. `.segmentationMask` is of type `ImageBitmap` with the same dimensions as the video stream, which you can use to copy onto a canvas:

```js
canvas = document.querySelector('#myCanvas')

handsfree = new Handsfree({pose: {
  enabled: true,
  enableSegmentation: true
}})

handsfree.use('greenscreen', data => {
  if (!data.pose) return

  canvas.clearRect(0, 0, handsfree.debug.$video.width, handsfree.debug.$video.height)
  canvas.image(data.pose.segmentationMask, 0, 0)
})
```

<video src="https://google.github.io/mediapipe/images/mobile/pose_segmentation.mp4" muted autoplay loop></video>
<br>
([video source](https://google.github.io/mediapipe/solutions/pose.html#segmentation_mask))


### Examples of accessing the data

```js
handsfree = new Handsfree({pose: true})
handsfree.start()

// From anywhere
handsfree.data.pose.poseLandmarks

// From inside a plugin
handsfree.use('logger', data => {
  if (!data.pose) return

  console.log(data.pose.poseLandmarks)
})

// From an event
document.addEventListener('handsfree-data', event => {
  const data = event.detail
  if (!data.pose) return

  console.log(data.pose.poseLandmarks)
})
```


## Examples

<!-- 🙌 Hi! If you'd like to add your project, just uncomment below with and replace the ALL_CAPS to your info. Thanks so much 🙏 -->

<!--
<div class="row">
  <div class="col-6">
    <Window title="DEMO_TITLE" :maximize="true">
      <div>
        <a href="LINK_TO_DEMO"><img alt="SHORT_DESCRIPTION" src="LINK_TO_GIPHY_OR_OTHER_PUBLIC_GIF_URL"></a>
      </div>
      <p>A_BRIEF_DESCRIPTION</p>
      <div>
        <ul>
          <li><a href="LINK_TO_DEMO">Try it on Glitch</a></li>
          <li><a href="LINK_TO_SOURCE_OR_GITHUB">See the source</a></li>
        </ul>
      </div>
    </Window>
  </div>
</div>
-->

<div class="row align-top">
  <div class="col-6">
    <Window title="Flappy Pose" :maximize="true">
      <section>
        <div>
          <router-link to="/ref/plugin/pinchScroll/"><img alt="Person playing Flappy Bird by flapping their arms. Flappy Bird is a game where you must flap the birds wings to fly or dodge barriers" src="https://media3.giphy.com/media/gUHHKdnuOW4OGOXcrI/giphy.gif"></router-link>
        </div>
        <p>In this game, the goal is to flap your arms to get the bird to fly around dodging obstacles. Made with an older version of Handsfree, but the API is very similar!</p>
        <div>
          <ul>
            <li><a href="https://flappy-pose.glitch.me/">Try it on Glitch</a></li>
            <li><a href="https://glitch.com/edit/#!/flappy-pose?path=src%2Fflap.js%3A32%3A4">See the source</a></li>
          </ul>
        </div>
      </section>
    </Window>
  </div>
  <div class="col-6">
    <Window title="Add your project">
      If you've made something with this model I'd love to showcase it here! DM me <a href="https://twitter.com/midiblocks">on Twitter</a>, <a class="https://github.com/midiblocks/handsfree/edit/master/docs/ref/model/hands.md">make a pull request</a>, or <a href="https://discord.gg/JeevWjTEdu">find us on Discord</a>.  
    </Window>
  </div>
</div>

<!-- Code -->
<script>
export default {
  data () {
    return {
      demoOpts: {
        autostart: true,

        weboji: false,
        hands: false,
        facemesh: false,
        pose: true,
        handpose: false
      }
    }
  },

  methods: {
    /**
     * Start the page with our preset options
     */
    startDemo () {
      this.$root.handsfree.update(this.demoOpts)
    }
  }
}
</script>