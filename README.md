# PiciosSymfonyLib
Useful PHP Symfony ^4 features

First add to your Controller class some required uses:

```php
use Doctrine\ORM\EntityManager;
use Symfony\Bundle\FrameworkBundle\Controller\Controller;
use Symfony\Component\HttpFoundation\JsonResponse;
use Symfony\Component\HttpFoundation\Request;
```
...

and then here's some useful Symfony 4 Controller members functions:

...
```php

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

  /*
   * redirects to the previous location
   */
	protected function back(Request $request, $default = 'index')
	{
		$referer = $request->headers->get('referer');
		//dump($referer); die;
		if ($referer) {
			return $this->redirect($referer);
		} else {
			return $this->redirect($default);
		}
	}

  /*
   * add flash message to further show it to user
   */
	protected function flash($message, $bag = 'success')
	{
		$flashbag = $this->get('session')->getFlashBag();
		$flashbag->add($bag, $message);
	}
	
  /*
   * returns json response with additional status and errors fields
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

```
...
