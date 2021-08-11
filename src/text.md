한국어번역

소개
입자는 정확히 그 이름에서 기대하는 것입니다. 그들은 매우 인기가 있으며 별, 연기, 비, 먼지, 불 및 기타 여러 가지와 같은 다양한 효과를 얻는 데 사용할 수 있습니다.

파티클의 좋은 점은 합리적인 프레임 속도로 화면에 수십만 개의 파티클을 표시할 수 있다는 것입니다. 단점은 각 입자가 항상 카메라를 향하는 평면(두 개의 삼각형)으로 구성된다는 것입니다.

파티클을 만드는 것은 Mesh 를 만드는 것만큼 간단 합니다. 우리는 필요 BufferGeometry , 입자를 처리 할 수있는 물질 ( PointsMaterial을 ) 대신에 생산의 메쉬 우리는 만들 필요가 포인트 .

설정
스타터는 장면의 중간에 있는 큐브로만 구성됩니다. 이 큐브는 모든 것이 제대로 작동하는지 확인합니다.

/assets/lessons/17/step-01.png

첫 번째 입자
큐브를 없애고 입자로 구성된 구체를 만들어 시작합시다.

기하학
기본 Three.js 지오메트리를 사용할 수 있습니다. Mesh와 같은 이유로 BufferGeometries 를 사용하는 것이 좋습니다 . 지오메트리의 각 정점은 입자가 됩니다.

/\*\*

- Particles
  \*/
  // Geometry
  const particlesGeometry = new THREE.SphereGeometry(1, 32, 32)
  자바스크립트
  포인트소재
  PointsMaterial 이라는 특별한 유형의 재료가 필요합니다 . 이 재료는 이미 많은 작업을 수행할 수 있지만 향후 수업에서 더 나아가기 위해 우리 자신의 입자 재료를 만드는 방법을 발견할 것입니다.

PointsMaterial는 등의 입자에 여러 속성의 특정이 size모든 입자의 크기를 제어하고 sizeAttenuation먼 입자가 가까운 입자보다 작아야합니다 경우 지정을 :

// Material
const particlesMaterial = new THREE.PointsMaterial({
size: 0.02,
sizeAttenuation: true
})
자바스크립트
항상 그렇듯이 재료를 생성한 후 이러한 속성을 변경할 수도 있습니다.

const particlesMaterial = new THREE.PointsMaterial()
particlesMaterial.size = 0.02
particlesMaterial.sizeAttenuation = true
자바스크립트
포인트들
마지막으로 Mesh 를 만드는 것과 같은 방식으로 최종 입자를 만들 수 있지만 이번에는 Points 클래스 를 사용합니다 . 장면에 추가하는 것을 잊지 마십시오.

// Points
const particles = new THREE.Points(particlesGeometry, particlesMaterial)
scene.add(particles)
자바스크립트
/assets/lessons/17/step-02.png

그것은 쉽다. 해당 입자를 사용자 지정해 보겠습니다.

커스텀 지오메트리
사용자 지정 기하 도형을 만들려면 BufferGeometry 에서 시작 position하여 도형 단원 에서 했던 것처럼 속성을 추가 할 수 있습니다 . 바꾸기 SphereGeometry 사용자 정의 지오메트리를하고 추가 'position'우리가 이전과 같은 속성을 :

// Geometry
const particlesGeometry = new THREE.BufferGeometry()
const count = 500

const positions = new Float32Array(count \* 3) // Multiply by 3 because each position is composed of 3 values (x, y, z)

for(let i = 0; i < count _ 3; i++) // Multiply by 3 for same reason
{
positions[i] = (Math.random() - 0.5) _ 10 // Math.random() - 0.5 to have a random value between -0.5 and +0.5
}

particlesGeometry.setAttribute('position', new THREE.BufferAttribute(positions, 3)) // Create the Three.js BufferAttribute and specify that each information is composed of 3 values
자바스크립트
/assets/lessons/17/step-03.png

이 코드를 스스로 꺼낼 수 없다고 좌절하지 마십시오. 조금 복잡하고 변수가 이상한 형식을 사용하고 있습니다.

장면 전체에 많은 입자를 가져와야 합니다. 지금이 즐거운 시간을 보내고 컴퓨터의 한계를 테스트할 수 있는 좋은 시간입니다. 시도 5000, 50000, 500000아마도. 수백만 개의 입자를 가질 수 있으며 여전히 적절한 프레임 속도를 가질 수 있습니다.

한계가 있다고 상상할 수 있습니다. 열등한 컴퓨터나 스마트폰에서는 수백만 개의 입자로 60fps 경험을 할 수 없습니다. 또한 프레임 속도를 크게 줄이는 효과를 추가할 예정입니다. 하지만 그래도 꽤 인상적입니다.

지금은 개수를 로 유지 5000하고 크기를 0.1다음 과 같이 변경해 보겠습니다 .

const count = 5000

// ...

particlesMaterial.size = 0.1

// ...
자바스크립트
/assets/lessons/17/step-04.png

색상, 맵 및 알파 맵
PointsMaterial 의 color속성을 사용하여 모든 입자의 색상을 변경할 수 있습니다 . 머티리얼을 인스턴스화한 후 이 속성을 변경하는 경우 Color 클래스 를 사용해야 한다는 것을 잊지 마십시오 .

particlesMaterial.color = new THREE.Color('#ff88cc')
자바스크립트
map속성을 사용하여 해당 입자에 텍스처를 넣을 수도 있습니다 . 코드에 이미 있는 TextureLoader를 사용하여 에 있는 텍스처 중 하나를 로드합니다 /static/textures/particles/.

/\*\*

- Textures
  \*/
  const textureLoader = new THREE.TextureLoader()
  const particleTexture = textureLoader.load('/textures/particles/2.png')

// ...

particlesMaterial.map = particleTexture
자바스크립트
/assets/lessons/17/step-05.png

이 텍스처는 Kenney에서 제공하는 팩의 크기가 조정된 버전이며 여기에서 전체 팩을 찾을 수 있습니다: https://www.kenney.nl/assets/particle-pack . 그러나 직접 만들 수도 있습니다.

보시다시피 color속성은 다른 재료와 마찬가지로 맵을 변경합니다.

자세히 보면 전면 입자가 후면 입자를 숨기고 있음을 알 수 있습니다.

다음 대신 속성 transparent의 텍스처를 사용하여 투명도를 활성화해야 alphaMap합니다 map.

// particlesMaterial.map = particleTexture
particlesMaterial.transparent = true
particlesMaterial.alphaMap = particleTexture
자바스크립트
/assets/lessons/17/step-07.png

이제 더 나아졌지만 여전히 입자의 일부 가장자리를 무작위로 볼 수 있습니다.

이는 파티클이 생성된 것과 동일한 순서로 그려지고 WebGL이 어느 쪽이 다른 쪽 앞에 있는지 실제로 알지 못하기 때문입니다.

이 문제를 해결하는 방법에는 여러 가지가 있습니다.

alphaTest 사용
는 alphaTest사이의 값입니다 0및 1그 픽셀의 투명도에 따라 픽셀을 렌더링하지 않을 때는 WebGL을 알 수 있습니다. 기본적으로 값은 0픽셀이 어쨌든 렌더링됨을 의미합니다. 와 같은 작은 값을 사용하는 0.001경우 알파가 0다음 과 같은 경우 픽셀이 렌더링되지 않습니다 .

particlesMaterial.alphaTest = 0.001
자바스크립트
이 솔루션은 완벽하지 않으며 자세히 살펴보면 여전히 글리치를 볼 수 있지만 이미 더 만족스럽습니다.

깊이 테스트 사용
그릴 때 WebGL은 그려지는 것이 이미 그려진 것보다 가까운지 테스트합니다. 이를 깊이 테스트라고 하며 비활성화할 수 있습니다(설명할 수 있음 alphaTest).

// particlesMaterial.alphaTest = 0.001
particlesMaterial.depthTest = false
자바스크립트
이 솔루션이 문제를 완전히 해결하는 것처럼 보이지만 깊이 테스트를 비활성화하면 장면에 다른 개체가 있거나 다른 색상의 입자가 있는 경우 버그가 발생할 수 있습니다. 입자는 장면의 나머지 부분 위에 있는 것처럼 그려질 수 있습니다.

장면에 큐브를 추가하여 다음을 확인하십시오.

const cube = new THREE.Mesh(
new THREE.BoxGeometry(),
new THREE.MeshBasicMaterial()
)
scene.add(cube)
자바스크립트
depthWrite 사용
우리가 말했듯이 WebGL은 그려지는 것이 이미 그려진 것보다 더 가까운지 테스트하고 있습니다. 그려지는 것의 깊이는 우리가 깊이 버퍼라고 부르는 것에 저장됩니다. 입자가 이 깊이 버퍼에 있는 것보다 더 가까운지 테스트하지 않는 대신 WebGL에 해당 깊이 버퍼에 입자를 쓰지 않도록 지시할 수 있습니다(설명할 수 있음 depthTest).

// particlesMaterial.alphaTest = 0.001
// particlesMaterial.depthTest = false
particlesMaterial.depthWrite = false
자바스크립트
우리의 경우 이 솔루션을 사용하면 거의 문제가 없는 문제를 해결할 수 있습니다. 때로는 투명도, 장면에 개체를 추가한 순서 등과 같은 많은 요소에 따라 입자 뒤 또는 앞에 다른 개체가 그려질 수 있습니다.

우리는 여러 기술을 보았지만 완벽한 솔루션은 없습니다. 프로젝트에 따라 최적의 조합을 찾아 적응해야 합니다.

블렌딩
현재 WebGL은 픽셀 하나를 다른 픽셀 위에 그립니다.

blending속성 을 변경하여 WebGL에 픽셀을 그릴 뿐만 아니라 이미 그려진 픽셀의 색상에 해당 픽셀의 색상 을 추가 하도록 지시할 수 있습니다 . 그것은 놀랍게 보일 수 있는 채도 효과를 가질 것입니다.

이를 테스트하려면 blending속성을 THREE.AdditiveBlending( depthWrite속성 유지)로 변경 하기만 하면 됩니다.

// particlesMaterial.alphaTest = 0.001
// particlesMaterial.depthTest = false
particlesMaterial.depthWrite = false
particlesMaterial.blending = THREE.AdditiveBlending
자바스크립트
/assets/lessons/17/step-13.png

20000이 효과를 더 잘 즐기려면 더 많은 입자를 추가합니다(예를 들어 ).

/assets/lessons/17/step-14.png

그러나 주의하십시오. 이 효과는 성능에 영향을 미치므로 60fps에서 이전만큼 많은 입자를 가질 수 없습니다.

이제 제거할 수 있습니다 cube.

/assets/lessons/17/step-15.png

다른 색상
각 입자에 대해 다른 색상을 가질 수 있습니다. 먼저 color위치에 대해 이름을 지정한 새 속성을 추가해야 합니다 . 색상은 빨강, 초록, 파랑(3가지 값)으로 구성되어 코드가 position속성 과 매우 유사 합니다. 실제로 다음 두 속성에 대해 동일한 루프를 사용할 수 있습니다.

const positions = new Float32Array(count _ 3)
const colors = new Float32Array(count _ 3)

for(let i = 0; i < count _ 3; i++)
{
positions[i] = (Math.random() - 0.5) _ 10
colors[i] = Math.random()
}

particlesGeometry.setAttribute('position', new THREE.BufferAttribute(positions, 3))
particlesGeometry.setAttribute('color', new THREE.BufferAttribute(colors, 3))
자바스크립트
단수와 복수에 주의하세요.

정점 색상을 활성화하려면 vertexColors속성을 true다음과 같이 변경하면 됩니다 .

particlesMaterial.vertexColors = true
자바스크립트
/assets/lessons/17/step-16.png

재료의 기본 색상은 여전히 ​​이러한 정점 색상에 영향을 줍니다. 색상을 변경하거나 댓글을 달아주세요.

// particlesMaterial.color = new THREE.Color('#ff88cc')
자바스크립트
/assets/lessons/17/step-17.png

생명 있는
입자에 애니메이션을 적용하는 방법에는 여러 가지가 있습니다.

포인트를 객체로 사용하여
Points 클래스는 Object3D 클래스 에서 상속 되기 때문에 원하는 대로 점을 이동, 회전 및 크기 조정할 수 있습니다.

tick함수 에서 입자 회전 :

const tick = () =>
{
const elapsedTime = clock.getElapsedTime()

    // Update particles
    particles.rotation.y = elapsedTime * 0.2

    // ...

}
자바스크립트
이것은 이미 멋지지만 우리는 각 입자에 대한 더 많은 제어를 원합니다.

속성을 변경하여
또 다른 솔루션은 각 정점 위치를 개별적으로 업데이트하는 것입니다. 이런 식으로 정점은 다른 궤적을 가질 수 있습니다. 입자가 파도 위에 떠 있는 것처럼 애니메이션을 적용할 것이지만 먼저 정점을 업데이트하는 방법을 살펴보겠습니다.

전체에 대해 수행한 이전 회전에 대해 주석을 달아 시작합니다 particles.

const tick = () =>
{
// ...

    // particles.rotation.y = elapsedTime * 0.2

    // ...

}
자바스크립트
각 꼭짓점을 업데이트하려면 position속성 의 오른쪽 부분을 업데이트해야 합니다. 모든 꼭짓점이 이 1차원 배열에 저장되기 때문입니다. 여기서 처음 3개의 값은 x, y및 z첫 번째 꼭짓점의 좌표에 해당하고 다음 3개 값은 에 해당합니다. x, y그리고 z두 번째 정점 등

우리는 정점만 위아래로 움직이기를 원합니다. 즉, y축만 업데이트할 것 입니다. position속성은 1차원 배열 이기 때문에 3 x 3으로 이동하고 y좌표인 두 번째 값만 업데이트해야 합니다.

각 정점을 살펴보는 것으로 시작하겠습니다.

const tick = () =>
{
// ...

    for(let i = 0; i < count; i++)
    {
        const i3 = i * 3
    }

    // ...

}
자바스크립트
여기 for에서 0로 가는 간단한 루프를 선택하고 단순히 3을 곱하여 3 x 3으로 가는 변수를 내부에 count만들었습니다 .i3i

파도의 움직임을 시뮬레이션하는 가장 쉬운 방법은 단순 부비동 을 사용하는 것 입니다. 먼저 모든 정점을 업데이트하여 동일한 주파수에서 위아래로 이동합니다.

y인덱스의 배열에 액세스 할 수있는 좌표 i3 + 1:

const tick = () =>
{
// ...

    for(let i = 0; i < count; i++)
    {
        const i3 = i * 3

        particlesGeometry.attributes.position.array[i3 + 1] = Math.sin(elapsedTime)
    }

    // ...

}
자바스크립트
불행히도 아무것도 움직이지 않습니다. 문제는 Three.js가 지오메트리가 변경되었음을 알려야 한다는 것입니다. 그렇게 하려면 정점 업데이트가 완료되면 속성 에 를 설정해야 needsUpdate합니다 .trueposition

const tick = () =>
{
// ...

    for(let i = 0; i < count; i++)
    {
        const i3 = i * 3

        particlesGeometry.attributes.position.array[i3 + 1] = Math.sin(elapsedTime)
    }
    particlesGeometry.attributes.position.needsUpdate = true

    // ...

}
자바스크립트
모든 입자는 평면처럼 위아래로 움직여야 합니다.

좋은 시작이고 거의 다 왔습니다. 이제 우리가 해야 할 일은 입자 사이의 부비동에 오프셋을 적용하여 파형을 얻는 것뿐입니다.

이를 위해 x좌표를 사용할 수 있습니다 . 그리고 우리는 우리가 사용하는 것과 동일한 기술을 사용하여이 값을 얻기 위해 y조정 대신의를 i3 + 1, 그것은 단지 i3:

const tick = () =>
{
// ...

    for(let i = 0; i < count; i++)
    {
        let i3 = i * 3

        const x = particlesGeometry.attributes.position.array[i3]
        particlesGeometry.attributes.position.array[i3 + 1] = Math.sin(elapsedTime + x)
    }
    particlesGeometry.attributes.position.needsUpdate = true

    // ...

}
자바스크립트
입자의 아름다운 파도를 얻어야 합니다. 불행히도 이 기술은 피해야 합니다. 20000입자 가 있는 경우 각 입자를 살펴보고 새 위치를 계산하고 각 프레임의 전체 속성을 업데이트합니다. 적은 수의 입자로 작동할 수 있지만 수백만 개의 입자가 필요합니다.

커스텀 셰이더를 사용하여
좋은 프레임 속도로 각 프레임에서 수백만 개의 입자를 업데이트하려면 자체 셰이더를 사용하여 자체 재질을 만들어야 합니다. 그러나 셰이더는 나중 학습을 위한 것입니다.
