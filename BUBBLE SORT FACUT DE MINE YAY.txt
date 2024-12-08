include win64.inc 
option win64:0111b
option literals:on ;posibil de aici era eroarea 
option SWITCHSTYLE: CSTYLE

.data
v1 db 4, 3, 2, 1
v2 word	1, 2, 3, 4
v3 dword 1, 2, 3, 4
v4 qword 1, 2, 3, 4
v_1 db -1, 2, -3, 4
v_2 word -1, 2, 3, 4
v_3 dword -1, 2, 3, 4
v_4 qword -1, 2, 3, 4

nr_elemente qword 4

tip1 qword 1
tip2 qword 2
tip3 qword 4
tip4 qword 8
tip_1 qword -1
tip_2 qword -2
tip_3 qword -4
tip_4 qword -8

ord_asc db 0
ord_dsc db 1

.code 
main proc
	sub rsp,32
	
	;FULLY FUNCTIONAL

	lea rcx, v_1
	mov rdx, nr_elemente
	mov r8,0
	
	mov r8, tip_1
	
	cmp r8,0
	jl negative
	jmp multi
	negative:
		neg r8

	multi:
		push rax
		mov rax, rdx
	    

		mul r8
		neg r8
		mov rdx,rax
		
		pop rax


	mov r9,0
	mov r9b, ord_asc
	
	sub rsp, 16
	call bubble
	add rsp, 16

	mov r12,0
	mov r13,0
	mov r14,0
	mov r15,0

	mov r12b, sbyte ptr [rcx]
	mov r13b, sbyte ptr [rcx+1]
	mov r14b, sbyte ptr [rcx+2]
	mov r15b ,sbyte ptr [rcx+3]

	

	mov rax,0
	mov rcx,0
	mov rbx,0 
	mov r8 ,0
	mov r9 ,0


	printf("%d %d %d %d", r12b,r13b,r14b,r15b)

	
	exit(0)
	add rsp,32
	ret
main endp
bubble proc uses rbx r13 r14 r15 
	sub rsp,32+16+32+256
	;rcx -adr rdx -nrelem r8 -tip date (cu sau fara semn byte) r9
	;rbx counter  r13 condition r14 auxiliary
	;known errors .if macro nu stie sa compare signed  SOL: folosim etichete si cmp jl jg
	
	.switch r8
		.case -8
			.if r9b == 0 
				mov r13b,0
				.while r13b ==  0
				
					mov r13b,1
					.for (rbx = 8 : rbx < rdx : rbx+=8)
						mov r14, sqword ptr[rcx + rbx -8]
						mov r15, sqword ptr[rcx + rbx ]
						
						;.if r14 > r15
						cmp r15,r14
						jl inter8as
						jmp end8as						
						inter8as:
								mov [rcx + rbx -8], r15
								mov [rcx +rbx], r14
						
								mov r13b, 0
						end8as:
						;.endif
						
					.endfor
				.endw
			
			.else 
				mov r13b,0
				.while r13b ==  0
				
					mov r13b,1
					.for (rbx = 8 : rbx < rdx : rbx+=8)
						mov r14, sqword ptr[rcx + rbx -8]
						mov r15, sqword ptr[rcx + rbx ]
						
						;.if r14 < r15
						cmp r14,r15
						jl inter8des
						jmp end8des						
						inter8des:
								mov [rcx + rbx -8], r15
								mov [rcx +rbx], r14
						
								mov r13b, 0
						end8des:
						;.endif	
					.endfor
				.endw
			.endif
		.break


		.case -4
			.if r9b == 0 
				mov r13b,0
				.while r13b ==  0
				
					mov r13b,1
					.for (rbx = 4 : rbx < rdx : rbx+=4)
						mov r14d, sdword ptr[rcx + rbx -4]
						mov r15d, sdword ptr[rcx + rbx ]
						
						;.if r14d > r15d
						cmp r15d,r14d
						jl inter4as
						jmp end4as
						inter4as:
								mov [rcx + rbx -4], r15d
								mov [rcx +rbx], r14d
								
								mov r13b, 0
						end4as:
						;.endif	
					.endfor
				.endw
			
			.else 
				mov r13b,0
				.while r13b ==  0
				
					mov r13b,1
					.for (rbx = 4 : rbx < rdx : rbx+=4)
						mov r14d, sdword ptr[rcx + rbx -4]
						mov r15d, sdword ptr[rcx + rbx ]
						
						;.if r14d < r15d
						cmp r14d,r15d
						jl inter4des
						jmp end4des
						inter4des:
								mov [rcx + rbx -4], r15d
								mov [rcx +rbx], r14d
								
								mov r13b, 0
						end4des:
						;.endif	
					.endfor
				.endw
			.endif
		.break
		
		
		.case -2
			.if r9b == 0 
				mov r13b,0
				.while r13b ==  0
				
					mov r13b,1
					.for (rbx = 2 : rbx < rdx : rbx+=2)
						mov r14w, sword ptr[rcx + rbx -2]
						mov r15w, sword ptr[rcx + rbx ]
						
						;.if r14w > r15w
						cmp r15w,r14w
						jl inter2as
						jmp end2as
						inter2as:
								mov [rcx + rbx -2], r15w
								mov [rcx +rbx], r14w
								
								mov r13b, 0
						end2as:
						;.endif	
					.endfor
				.endw
			
			.else 
				mov r13b,0
				.while r13b ==  0
				
					mov r13b,1
					.for (rbx = 2 : rbx < rdx : rbx+=2)
						mov r14w, sword ptr[rcx + rbx -2]
						mov r15w, sword ptr[rcx + rbx ]
						
						;.if r14w < r15w
						cmp r14w,r15w
						jl inter2des
						jmp end2des
						inter2des:
								mov [rcx + rbx -2], r15w
								mov [rcx +rbx], r14w
								
								mov r13b, 0
						end2des:
						;.endif	
					.endfor
				.endw
			.endif
		.break
		

		.case -1 
			.if r9b == 0 
				mov r13b,0
				.while r13b ==  0
				
					mov r13b,1
					.for (rbx = 1 : rbx < rdx : rbx++)
						mov r14b, sbyte ptr[rcx + rbx -1]
						mov r15b, sbyte ptr[rcx + rbx ]
						
						;.if r14b > r15b
						cmp r15b,r14b
						jl inter1as
						jmp end1as
						inter1as:
								mov [rcx + rbx -1], r15b
								mov [rcx +rbx], r14b
								
								mov r13b, 0
						end1as:
						;.endif	
					.endfor
				.endw
			
			.else 
				mov r13b,0
				.while r13b ==  0
				
					mov r13b,1
					.for (rbx = 1 : rbx < rdx : rbx++)
						mov r14b, sbyte ptr[rcx + rbx -1]
						mov r15b, sbyte ptr[rcx + rbx ]
						
						;.if r14b < r15b
						cmp r14b,r15b
						jl inter1des
						jmp end1des
						inter1des:
								mov [rcx + rbx -1], r15b
								mov [rcx +rbx], r14b
								
								mov r13b, 0
						end1des:
						;.endif	
					.endfor
				.endw
			.endif
		.break
		

		.case 1	
			.if r9b == 0 
				mov r13b,0
				.while r13b ==  0
				
					mov r13b,1
					.for (rbx = 1 : rbx < rdx : rbx++)
						mov r14b, byte ptr[rcx + rbx -1]
						mov r15b, byte ptr[rcx + rbx ]
						
						.if r14b > r15b
								
								mov [rcx + rbx -1], r15b
								mov [rcx +rbx], r14b
								
								mov r13b, 0
						.endif	
					.endfor
				.endw
			
			.else 
				mov r13b,0
				.while r13b ==  0
				
					mov r13b,1
					.for (rbx = 1 : rbx < rdx : rbx++)
						mov r14b, byte ptr[rcx + rbx -1]
						mov r15b, byte ptr[rcx + rbx ]
						
						.if r14b < r15b
								
								mov [rcx + rbx -1], r15b
								mov [rcx +rbx], r14b
								
								mov r13b, 0
				
						.endif	
					.endfor
				.endw
			.endif

			.break
		.case 2
			.if r9b == 0 
				mov r13b,0
				.while r13b ==  0
				
					mov r13b,1
					.for (rbx = 2 : rbx < rdx : rbx+=2)
						mov r14w, word ptr[rcx + rbx -2]
						mov r15w, word ptr[rcx + rbx ]
						
						.if r14w > r15w
								
								mov [rcx + rbx -2], r15w
								mov [rcx +rbx], r14w
								
								mov r13b, 0
						.endif	
					.endfor
				.endw
			
			.else 
				mov r13b,0
				.while r13b ==  0
				
					mov r13b,1
					.for (rbx = 2 : rbx < rdx : rbx+=2)
						mov r14w, word ptr[rcx + rbx -2]
						mov r15w, word ptr[rcx + rbx ]
						
						.if r14w < r15w
								
								mov [rcx + rbx -2], r15w
								mov [rcx +rbx], r14w
								
								mov r13b, 0
						.endif	
					.endfor
				.endw
			.endif
		.break

		
		.case 4
			.if r9b == 0 
				mov r13b,0
				.while r13b ==  0
				
					mov r13b,1
					.for (rbx = 4 : rbx < rdx : rbx+=4)
						mov r14d, dword ptr[rcx + rbx -4]
						mov r15d, dword ptr[rcx + rbx ]
						
						.if r14d > r15d
								
								mov [rcx + rbx -4], r15d
								mov [rcx +rbx], r14d
								
								mov r13b, 0
						.endif	
					.endfor
				.endw
			
			.else 
				mov r13b,0
				.while r13b ==  0
				
					mov r13b,1
					.for (rbx = 4 : rbx < rdx : rbx+=4)
						mov r14d, dword ptr[rcx + rbx -4]
						mov r15d, dword ptr[rcx + rbx ]
						
						.if r14d < r15d
								
								mov [rcx + rbx -4], r15d
								mov [rcx +rbx], r14d
								
								mov r13b, 0
						.endif	
					.endfor
				.endw
			.endif
		.break


		.case 8
			.if r9b == 0 
				mov r13b,0
				.while r13b ==  0
				
					mov r13b,1
					.for (rbx = 8 : rbx < rdx : rbx+=8)
						mov r14, qword ptr[rcx + rbx -8]
						mov r15, qword ptr[rcx + rbx ]
						
						.if r14 > r15
								
								mov [rcx + rbx -8], r15
								mov [rcx +rbx], r14
								
								mov r13b, 0
						.endif	
					.endfor
				.endw
			
			.else 
				mov r13b,0
				.while r13b ==  0
				
					mov r13b,1
					.for (rbx = 8 : rbx < rdx : rbx+=8)
						mov r14, qword ptr[rcx + rbx -8]
						mov r15, qword ptr[rcx + rbx ]
						
						.if r14 < r15
								
								mov [rcx + rbx -8], r15
								mov [rcx +rbx], r14
								
								mov r13b, 0
						.endif	
					.endfor
				.endw
			.endif
		.break



		.default
			mov rax,0
		.break
	.endsw


	add rsp,32+16+32+256
	ret
bubble endp
end 