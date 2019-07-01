# PiciosSymfonyLib
Useful PHP Symfony ^4 features

First add to your Controller class some required uses:

```php
use App\LibBundle\Lib\Breadcrumb\Breadcrumb;
use Doctrine\ORM\EntityManager;
use Symfony\Bundle\FrameworkBundle\Controller\Controller;
use Symfony\Component\HttpFoundation\JsonResponse;
use Symfony\Component\HttpFoundation\RedirectResponse;
use Symfony\Component\HttpFoundation\Request;
...
```

and then here's some useful Symfony 4 Controller members functions:


```php
...

	/**
	 *
	 * @var EntityManager 
	 */
	protected $em = null;

	/**
	 * 
	 * @return EntityManager
	 */
	protected function getEm()
	{
		if ($this->em === null) {
			$this->em = $this->getDoctrine()->getManager();
		}
		return $this->em;
	}

	/**
	 * redirects to the previous address
	 * 
	 * @param Request $request
	 * @param string $default
	 * @return RedirectResponse
	 */
	protected function back(Request $request, $default = 'index')
	{
		$referer = $request->headers->get('referer');
		if ($referer) {
			return $this->redirect($referer);
		} else {
			return $this->redirect($default);
		}
	}

	/**
	 * ads a flash message to further show it to the user
	 * 
	 * @param string $message
	 * @param string $bag
	 */
	protected function flash(string $message, ?string $bag = 'success')
	{
		$flashbag = $this->get('session')->getFlashBag();
		$flashbag->add($bag, $message);
	}
	
	/**
	 * returns JsonResponse
	 * 
	 * @param array $resp
	 * @param array $errors
	 * @return JsonResponse
	 */
	protected function ajaxResponse(?array $resp = null, ?array $errors = [])
	{
		$rsp = [
			'status' => (bool) $errors,
			'errors' => $errors,
		];
		if ($resp) {
			$rsp = array_merge($rsp, $resp);
		}
		return new JsonResponse($rsp);
	}
	
...
```
