---
title: "두번째 글"
date: 2019-03-14 20:48:28 -0400
categories: exam update
---
여기에는 내용을 끄적이고
"``` ``` 이것은 주석 같은 것 인가?"
```java
@Controller()
@RequestMapping("/member/")
public class MemberController {

	/*@Inject
	private JavaMailSender mailSender;*/
	
	@Autowired
	private MemberService memberService;
	
	@RequestMapping("index")
	private String index(Principal principal)	{
	
		String memberId = principal.getName();
		/*Member member = memberService.getMemberNicName(memberId);
		System.out.println(member);
		System.out.println(member.getNicName());*/
		//System.out.println(memberId);
		String roleName = memberService.getDefaultMemberRole(memberId);
		//System.out.println(roleName);
		if(roleName.equals("ROLE_ACADEMY_ADMIN"))
			return "redirect:../admin/index";
		else if(roleName.equals("ROLE_ACADEMY_TEACHER"))
			return "redirect:../teacher/index";
		else
			return "redirect:../student/index";
		
	}
	
	@GetMapping("signup")
	private String signup() {
		
		return "member.signup";
		
	}
	
	@PostMapping("signup")
	private String signup(Member member, 
			String phone1, String phone2, String phone3,
			String email1, String email2) /*throws MessagingException*/ {
		
		PasswordEncoder encoder = new BCryptPasswordEncoder();
		String encodedPwd = encoder.encode(member.getPwd());
		member.setPwd(encodedPwd);
		/*System.out.println(phone1);
		System.out.println(phone2);
		System.out.println(phone3);*/
		String phone = phone1 + "-" + phone2 + "-" + phone3;
		/*System.out.println(phone);*/
		String email = email1 + "@" + email2;
		member.setPhone(phone);
		member.setEmail(email);
		
		memberService.insertMember(member);
		/*MailHandler sendMail = new MailHandler(mailSender);
		sendMail.setSubject("[이메일 인증]");
		sendMail.setText(new StringBuffer()
					.append("<h1>메일인증</h1>")
					.append("<a href='http'")
				);*/
		
		
		return "redirect:../index";
		
	}
	
	@RequestMapping(value = "idchk", method= RequestMethod.POST)
	@ResponseBody
	public String idchk(String useId) {
		//System.out.println("중복체크실행");
		String str = "";
		//System.out.println(useId);
		int count = memberService.idchk(useId);
		//System.out.println("중복ㅊ크 : " + count);
		if(count==1) {
			str = "NO";
		} else {
			str = "YES";
		}
		
		return str;
		
	}
	
	/*
	@GetMapping("edit")
	private String modify() {
		Member member = memberService.getMember(member.getId());
		return "member.modify";
	}
	@PostMapping("edit")
	private String modify(Member member) {
		
		PasswordEncoder encoder = new BCryptPasswordEncoder();
		String encodedPwd = encoder.encode(member.getPwd());
		member.setPwd(encodedPwd);
		
		memberService.updateMember(member);
		
		return "";
	}*/
	
	@GetMapping("login")
	private String login() {
		return "member.login";
	}
	
}
```

Check out the [Jekyll docs][jekyll-docs] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll Talk][jekyll-talk].

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
