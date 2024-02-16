## Step By Step Setup of QNATK

After adding submodule to your device , you can follow the steps below for setting up the project.

Create one file name `qnatk.controller.ts` in your `src` folder and add this as it is code to it there is no need to change:

```typescript
import {
    Body,
    Controller,
    Param,
    Post,
    UploadedFiles,
    UseGuards,
    UseInterceptors,
    Get,
  } from '@nestjs/common';
  import { QnatkListDTO } from 'src/qnatk/src/dto/QnatkListDTO';
  import {
    AnyFilesInterceptor,
    NoFilesInterceptor,
  } from '@nestjs/platform-express';
  import { ActionListDTO } from './qnatk/src';
  import { GetUser } from './auth/getuser.decorator';
  import { QnatkControllerService } from './qnatk/src';
  import { ApiParam } from '@nestjs/swagger';
  import { UserJWT } from './dto/user-jwt.dto';
  import { Express } from 'express';
  // import { JwtService } from '@nestjs/jwt';
  // import { AuthGuard } from './auth/auth.guard';
  
  @Controller('qnatk')
  // @UseGuards(AuthGuard)
  export class QnatkController {
    constructor(private qnatkControllerService: QnatkControllerService) {
      // private jwtService: JwtService
    }
  
    @Post(':baseModel/list')
    async findAll(
      // @Headers('authorization') authorization: string,
      @Param('baseModel') baseModel: string,
      @Body() body: QnatkListDTO,
      @GetUser() user: UserJWT,
    ) {
      return await this.qnatkControllerService.list(baseModel, body, user);
    }
  
    @Post(':baseModel/list-and-count')
    async findAndCountAll(
      // @Headers('authorization') authorization: string,
      @Param('baseModel') baseModel: string,
      @Body() body: QnatkListDTO,
      @GetUser() user: UserJWT,
    ) {
      return this.qnatkControllerService.listAndCount(baseModel, body, user);
    }
  
    @Post(':baseModel/create')
    @UseInterceptors(NoFilesInterceptor())
    async addNew(
      // @Headers('authorization') authorization: string,
      @Param('baseModel') baseModel: string,
      @Body() data: any,
      @GetUser() user: UserJWT,
    ) {
      // const token = authorization.replace("Bearer ", "");
      // const decodedToken = await this.jwtService.verifyAsync(token);
      // data.createdById = user.id;
      // data.updatedById = user.id;
      return await this.qnatkControllerService.addNew<UserJWT>(
        baseModel,
        data,
        user,
      );
    }
  
    @Post(':baseModel/create-with-files')
    @UseInterceptors(AnyFilesInterceptor())
    async addNewWithFile(
      // @Headers('authorization') authorization: string,
      @Param('baseModel') baseModel: string,
      @Body() body: any,
      @UploadedFiles() files: Array<Express.Multer.File>,
      @GetUser() user: UserJWT,
    ) {
      const data = { ...body };
      // data.createdById = user.id;
      // data.updatedById = user.id;
      return this.qnatkControllerService.addNewWithFile<UserJWT>(
        baseModel,
        data,
        files,
        user,
      );
    }
  
    @Post(':baseModel/actionExecute/:actionName/:skipModelLoad?')
    @ApiParam({ name: 'baseModel', type: String })
    @ApiParam({ name: 'actionName', type: String })
    async executeAction(
      // @Headers('authorization') authorization: string,
      @Param('baseModel') baseModel: string,
      @Param('actionName') action: any,
      @Body() body: { action: ActionListDTO; record: any },
      @GetUser() user: UserJWT,
      @Param('skipModelLoad') skipModelLoad?: boolean,
    ) {
      const data = { ...body };
      return this.qnatkControllerService.executeAction(
        baseModel,
        action,
        data,
        user,
        skipModelLoad,
      );
    }
  
    @Post(':baseModel/bulkActionExecute/:actionName')
    async bulkExecuteAction(
      // @Headers('authorization') authorization: string,
      @Param('baseModel') baseModel: string,
      @Param('actionName') action: any,
      @Body() body: { action: ActionListDTO; records: any },
      @GetUser() user: UserJWT,
    ) {
      const data = { ...body };
      return await this.qnatkControllerService.bulkExecuteAction<UserJWT>(
        baseModel,
        action,
        data,
        user,
      );
    }
  
    @Post(':baseModel/update/:primaryId/:primaryField?')
    @UseInterceptors(NoFilesInterceptor())
    async updateByPk(
      // @Headers('authorization') authorization: string,
      @Param('baseModel') baseModel: string,
      @Param('primaryId') primaryId: string | number,
      @Body() data: any,
      @GetUser() user: UserJWT,
      @Param('primaryField') primaryField?: string,
    ) {
      primaryField = primaryField || 'id';
      data.updatedById = user.id;
      return await this.qnatkControllerService.updateByPk(
        baseModel,
        primaryId,
        primaryField,
        data,
        user,
      );
    }
  
    @Get(':baseModel/delete/:primaryId/:primaryField?')
    @UseInterceptors(NoFilesInterceptor())
    async deleteByPk(
      // @Headers('authorization') authorization: string,
      @Param('baseModel') baseModel: string,
      @Param('primaryId') primaryId: string | number,
      @GetUser() user: UserJWT,
      @Param('primaryField') primaryField?: string,
    ) {
      primaryField = primaryField || 'id';
  
      return await this.qnatkControllerService.deleteByPk(
        baseModel,
        primaryId,
        primaryField,
        user,
      );
    }
  }
```

Now create a folder in your `src/auth` and add the file name `getuser.decorator.ts` the content of the file is below:

```typescript
import { createParamDecorator, ExecutionContext } from '@nestjs/common';

export const GetUser = createParamDecorator(
    (data: unknown, ctx: ExecutionContext) => {
        const request = ctx.switchToHttp().getRequest();
        return request.user;
    },
);
```

Now create folder in your `src/dto` and add the file name `user-jwt.dto.ts` the content of the file is below:

```typescript
export class  UserJWT {
    id:number;
}
```

Now replace your `app.module.ts` file code with this code:

```typescript
...

import { QnatkModule } from './qnatk/src/qnatk.module';
import { QnatkController } from './qnatk.controller';
import { HooksList } from './hooks/hooks-list';

...

@Module({
  imports: [
    
    ...

    SequelizeModule.forFeature(sequelizeModelArray),
    QnatkModule.forRoot(
      {
        hooksDirectory: __dirname + '/hooks',
        hookNameConstants: hookNameValues,
      },
      {
        models: sequelizeModelArray,
        actions: undefined
      },
      [AppModule],
    ),
  ],
  controllers: [... ,QnatkController, ...],
  providers: [AppService],
})
export class AppModule {}
```

!!! Information
    We have used `models: sequelizeModelArray` line of code because  we have multiple models in our application. We have passed it in an array so its easy to access all the models which you want to access for qnatk.





Now after all this steps now its time to run your project so simply write this command in your terminal:

```bash
npm run start:dev
```