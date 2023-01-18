<p align="center">
    <img width="160" src="./Images/Icon-1.png" alt="ScriptableAnimatorController">
    <h1 align="center">Scriptable Animator Controller</h1>
    <p align="center">
        <a href="https://assetstore.unity.com/packages/tools/animation/scriptable-animator-controller-240117" target="_blank"><img src="https://unity-assetstorev2-prd.storage.googleapis.com/cdn-origin/assets/as/views/common/components/Logo/src/unity-assetstore-logo-new.50ac708aeae28b8b6bf369ece5875fa5.svg" alt="Product" style="height: 32px !important;width: 200px !important;"></a>
    </p>
</p>

## ❓ What is the Scriptable Animator Controller?
This asset was created as an alternative to the mecanim animation system. When programming with Mecanim, the transition lines become more complex as the number of states increases. The Scriptable Animator Controller provides simple and effective control of state transitions with lines of code.

## 📦 Demo
Common uses of the asset are available at the github project link. I thought it would be appropriate to use 'Starter Assets' when promoting the product. One of the reasons I prefer 'Starter Assets' is that it can effectively display the importance of parameter values in the 'Play' method with 'Starter Assets'. If you think there is a better asset for the demo, you can contact me.

## 💲 Is it worth buying?
The promotional period provides a good opportunity to purchase the product at a discount. During this period, you can criticize the positive or negative aspects of the product by purchasing the product. You can help others with the success of the product. If you have a problem, you can follow the steps I described in the 'Reliability/Support' section.

##  📌 Reliability/Support
During the development process, I tried the asset with almost all scenarios and applied the necessary improvements. After making sure the asset was working properly, I grew the project. I took care that the methods had sufficient manageability in different scenarios. I did my best to make it error free. However, in case of errors or inadequacies in the methods, you can follow the steps below.
1. You can create a new issue from github issues and wait for it to be resolved.
2. Remember that you have the right to return the product.

## #️⃣ About source code;
99% of codes are readable and changeable. But I don't think you will need to change it.

## ⚙️ How does it work?
<details><summary>Basic Example</summary>

 ```c#
    public sealed class PlayerAnimatorController : AnimatorBehaviour
    {
        private int mainLayer;

        [SerializeField] private ScriptableSingleAnimation _animation;
        [NonSerialized] private SingleAnimation _dynamicState = SingleAnimation.Scriptable.GetInstance();

        private void Start()
        {
            // Create Layer.
            mainLayer = CreateLayer(weight: 1, stateCount: 1);
        }

        private void FixedUpdate()
        {
            // Play Animation.
            Play(mainLayer, _dynamicState, _animation);
        }
    }
```
</details>

<details><summary>Multi Layer Example</summary>

 ```c#
    public sealed class ExampleAnimatorController : AnimatorBehaviour
    {
        private int mainLayer, secondLayer;

        [Header("1.Layer")]
        [SerializeField] private ScriptableBlendTree1D _movement;
        [NonSerialized] private BlendTree1D _movementState = BlendTree1D.Scriptable.GetInstance();

        [Header("2.Layer")]
        [SerializeField] private AvatarMask _mask;
        [SerializeField] private ScriptableSingleAnimation _animation;
        [NonSerialized] private SingleAnimation _dynamicState = SingleAnimation.Scriptable.GetInstance();
        [SerializeField, Range(0, 1)] private float _layerWeight;
        [SerializeField, Range(0, 1)] private float _layerSpeed;

        private void Start()
        {
            // Create Layers.
            mainLayer = CreateLayer(weight: 1, stateCount: 1);
            secondLayer = CreateLayer(mask: _mask, weight: 0, stateCount: 1);
        }

        private void FixedUpdate()
        {
            // Main Layer
            Play(mainLayer, _movementState, _movement);

            // Second Layer
            if (GetLayerWeight(secondLayer) != _layerWeight) { SetLayerWeight(secondLayer, _layerWeight); }
            if (GetLayerSpeed(secondLayer) != _layerSpeed) { SetLayerSpeed(secondLayer, _layerSpeed); }

            if (GetLayerWeight(secondLayer) > 0)
            {
                PlayOver(secondLayer, _dynamicState, _animation);
            }
            else
            {
                SingleAnimation.Stop(_dynamicState);
            }
        }

        private void OnEnable() => Play();
        private void OnDisable() => Stop();
        protected override void OnDestroy()
        {
            base.OnDestroy();   //If the row is deleted it can cause a problem.
        }
    }
```
</details>