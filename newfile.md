## API ##

- Fast Gradient Sign Method
  (FGSM)
  [basic](https://arxiv.org/abs/1412.6572)/[iterative](https://arxiv.org/abs/1607.02533)

    ```python
    fgsm(model, x, eps=0.01, nb_epoch=1, clip_min=0.0, clip_max=1.0)
    ```

- [Target class Gradient Sign Method (TGSM)](https://arxiv.org/abs/1607.02533)

    ```python
tgsm(model, x, y=None, eps=0.01, nb_epoch=1, clip_min=0.0, clip_max=1.0)
    ```

    When `y=None`, this implements the least-likely class method.  If
    `y` is an integer or a list of integers, the source image is
    modified towards label `y`.

- [Jacobian-based Saliency Map Approach (JSMA)](https://arxiv.org/abs/1511.07528)

    ```python
jsma(model, x, y, nb_epoch=1.0, eps=1., clip_min=0.0, clip_max=1.0, pair=False, min_proba=0.0)
    ```

    `y` is the target label, could be an integer or a list.  when
    `nb_epoch` is a floating number in `[0, 1]`, it denotes the
    maximum distortion allowed and `nb_epoch` is automatically
    determined.  `min_proba` denotes the minimum confidence of target
    image.  If `pair=True`, then modifies two pixels at a time.

- Saliency map difference approach (SMDA)

    ```python
smda(model, x, y, nb_epoch=1.0, eps=1., clip_min=0.0, clip_max=1.0, min_proba=0.0)
    ```

    Similar to `jsma` interface.  The only difference is the saliency
    score calculation.  In `jsma`, saliency score is calculated as
    `dt/dx * (-do/dx)`, which in `smda`, the saliency score is
    `dt/dx - do/dx` which is more straightforward and simpler.

## Fun Examples ##
